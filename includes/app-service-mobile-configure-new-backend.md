
1. Klicken Sie auf **App Services**, wählen Sie Ihr mobiles App-Back-End aus, und klicken Sie auf **Schnellstart** und dann auf Ihre Clientplattform (iOS, Android, Xamarin, Cordova).

![Azure-Portal mit hervorgehobenem Mobile Apps-Schnellstart][quickstart]

2. Wenn die Verbindung mit der Datenbank nicht konfiguriert ist, müssen Sie eine Datenverbindung erstellen.

![Azure-Portal mit Mobile Apps – Verbindung mit BD herstellen][connect]

  * Erstellen Sie eine neue SQL-Datenbank und einen Server.

  ![Azure-Portal mit Mobile Apps – BD und Server neu erstellen][server]

  * Warten Sie, bis die Erstellung der Datenverbindung erfolgreich abgeschlossen wurde.

  ![Azure-Portal mit Mobile Apps – Benachrichtigung über Erstellung der Datenverbindung][notification]

  * Die Herstellung der Datenverbindung muss erfolgreich sein.

  ![Azure-Portal mit Mobile Apps – Benachrichtigung über Erstellung der Datenverbindung][already-connection]

3. Wählen Sie unter **2. Tabellen-API erstellen** als **Back-End-Sprache** die Option „Node.js“ aus. Akzeptieren Sie die Bestätigung, und klicken Sie auf **TodoItem-Tabelle erstellen**. Dadurch wird eine neue *TodoItem* -Tabelle in Ihrer Datenbank erstellt. Beachten Sie, dass durch Wechseln der Sprache für ein vorhandenes Back-End zu Node.js alle Inhalte überschrieben werden. Wenn Sie stattdessen ein .NET-Back-End erstellen möchten, hilft Ihnen [diese Anleitung][instructions] weiter.

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
