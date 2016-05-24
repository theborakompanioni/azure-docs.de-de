<properties
	pageTitle="Konfigurieren der Facebook-Authentifizierung für Ihre App Services-Anwendung"
	description="Erfahren Sie, wie Sie die Facebook-Authentifizierung für Ihre App Services-Anwendung konfigurieren."
	services="app-service"
	documentationCenter=""
	authors="mattchenderson"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="mahender"/>

# Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema erfahren Sie, wie Sie Azure App Services für die Verwendung von Facebook als Authentifizierungsanbieter konfigurieren.

Sie benötigen ein Facebook-Konto mit verifizierter E-Mail-Adresse und einer Mobiltelefonnummer, um den in diesem Thema beschriebenen Vorgang abzuschließen. Rufen Sie die Seite [facebook.com] auf, um ein neues Facebook-Konto zu erstellen.

## <a name="register"> </a>Registrieren Ihrer Anwendung für Facebook

1. Melden Sie sich beim [Azure-Portal] an, und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**. Diese verwenden Sie zur Konfiguration der Facebook-App.

2. Navigieren Sie in einem anderen Browserfenster zur Website für [Facebook-Entwickler], und melden Sie sich mit den Anmeldeinformationen für Ihr Facebook-Konto an.

3. (Optional) Falls Sie sich noch nicht registriert haben, klicken Sie auf **Apps** > **Register as a Developer**. Akzeptieren Sie dann die Richtlinie, und führen Sie die Registrierungsschritte durch.

4. Klicken Sie auf **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.

5. Geben Sie in **Display Name** einen eindeutigen Anzeigenamen für Ihre App ein, wählen Sie unter **Category** eine Kategorie für die App aus, klicken Sie dann auf **Create App ID**, und schließen Sie die Schritte für die Sicherheitsüberprüfung ab. Dadurch gelangen Sie zum Entwickler-Dashboard für Ihre neue Facebook-App.

6. Klicken Sie unter „Facebook Login“ auf **Get Started**. Fügen Sie den **Redirect URI** Ihrer Anwendung zu **Valid OAuth redirect URIs** hinzu, und klicken Sie dann auf **Save Changes**.

	> [AZURE.NOTE] Der Umleitungs-URI ist die URL Ihrer Anwendung, an die der Pfad _/.auth/login/facebook/callback_ angefügt wurde. Beispiel: `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.

6. Klicken Sie im linken Navigationsbereich auf **Settings**. Klicken Sie im Feld **App Secret** auf **Show**, geben Sie bei Aufforderung Ihr Kennwort ein, und notieren Sie die Werte für **App ID** und **App Secret**. Ihre Anwendung wird später in Azure mit diesen Angaben konfiguriert.

	> [AZURE.IMPORTANT] Der App-Schlüssel ist eine wichtige Anmeldeinformation. Teilen Sie diesen Schlüssel mit niemandem, und geben Sie ihn nicht über Ihre Anwendung weiter.

7. Das zum Registrieren der Anwendung verwendete Facebook-Konto fungiert als Administrator der App. Zu diesem Zeitpunkt können sich nur Administratoren bei der Anwendung anmelden. Um andere Facebook-Konten zu authentifizieren, klicken Sie auf **App Review**, und aktivieren Sie **Make <your-app-name> public**, um den allgemeinen öffentlichen Zugriff mithilfe der Facebook-Authentifizierung zu aktivieren.

## <a name="secrets"> </a>Hinzufügen von Facebook-Informationen zu Ihrer Anwendung

1. Navigieren Sie im [Azure-Portal] wieder zu Ihrer Anwendung. Klicken Sie auf **Einstellungen** > **Authentifizierung/Autorisierung**, und stellen Sie sicher, dass für die **App Service-Authentifizierung** die Option **Ein** festgelegt ist.

2. Klicken Sie auf **Facebook**, fügen Sie die App-ID und den geheimen App-Schlüssel ein, die Sie zuvor erhalten haben, aktivieren Sie optional alle Bereiche, die Ihre Anwendung benötigt, und klicken Sie dann auf **OK**.

    ![][0]

	Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.

3. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Facebook authentifiziert werden, legen Sie **Auszuführende Aktion bei nicht authentifizierter Anforderung** auf **Facebook** fest. Dies erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Facebook umgeleitet.

4. Klicken Sie nach dem Konfigurieren der Authentifizierung auf **Speichern**.

Sie können nun Facebook für die Authentifizierung in Ihrer App verwenden.

## <a name="related-content"> </a>Verwandte Inhalte

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook-Entwickler]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /de-DE/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-Portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0511_2016-->