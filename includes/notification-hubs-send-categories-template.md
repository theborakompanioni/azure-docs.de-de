
In diesem Abschnitt erfahren Sie, wie Sie Neuigkeiten als Vorlage mit Tags von einer .NET-Konsolen-App senden.

Wenn Sie Mobile Apps verwenden, lesen Sie das Tutorial [Hinzufügen von Pushbenachrichtigungen zu Mobile Apps], und wählen Sie oben Ihre Plattform aus.

Wenn Sie Java oder PHP verwenden möchten, lesen Sie die Informationen unter [Gewusst wie: Verwenden von Notification Hubs von Java/PHP aus]. Sie können Benachrichtigungen von beliebigen Back-Ends mithilfe der [Notification Hubs-REST-Schnittstelle] senden.

Überspringen Sie die Schritte 1 bis 3, wenn Sie im Rahmen des Tutorials [Erste Schritte mit Notification Hubs] eine Konsolen-App für das Senden von Benachrichtigungen erstellt haben.

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolenanwendung
   
       ![][13]
2. Klicken Sie im Visual Studio-Hauptmenü auf **Extras**, **Bibliothekspaket-Manager** und **Paket-Manager-Konsole**. Geben Sie dann im Konsolenfenster Folgendes ein, und drücken Sie die **Eingabetaste**:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Dies fügt mithilfe des [Microsoft.Azure.NotificationHubs-NuGet-Pakets] einen Verweis auf das Azure Notification Hubs-SDK hinzu.
3. Öffnen Sie die Datei "Program.cs", und fügen Sie die folgende `using` -Anweisung hinzu:
   
        using Microsoft.Azure.NotificationHubs;
4. Fügen Sie in der `Program` -Klasse die folgende Methode hinzu, oder ersetzen Sie sie, falls sie bereits vorhanden ist:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Sending the notification as a template notification. All template registrations that contain
            // "messageParam" and the proper tags will receive the notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    Dieser Code sendet eine Vorlagenbenachrichtigung für jedes der sechs Tags im Zeichenfolgenarray. Durch die Verwendung von Tags wird sichergestellt, dass Geräte nur Benachrichtigungen für die registrierten Kategorien erhalten.
5. Ersetzen Sie im Code oben die Platzhalter `<hub name>` und `<connection string with full access>` durch den Namen Ihres Notification Hubs und die Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* aus dem Dashboard für Ihren Notification Hub.
6. Fügen Sie die folgenden Zeilen zur **Main** -Methode hinzu:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();
7. Erstellen Sie die Konsolenanwendung.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Erste Schritte mit Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs-REST-Schnittstelle]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Hinzufügen von Pushbenachrichtigungen zu Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[Gewusst wie: Verwenden von Notification Hubs von Java/PHP aus]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.NotificationHubs-NuGet-Pakets]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/


<!--HONumber=Nov16_HO3-->


