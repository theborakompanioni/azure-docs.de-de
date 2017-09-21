
In diesem Abschnitt senden Sie Neuigkeiten als Vorlagenbenachrichtigungen mit Tags über eine .NET-Konsolen-App.

Wenn Sie das Mobile Apps-Feature von Microsoft Azure App Service verwenden, ist es ratsam, das Tutorial [Hinzufügen von Pushbenachrichtigungen zu Mobile Apps] durchzuarbeiten und im oberen Bereich Ihre Plattform auszuwählen.

Wenn Sie Java oder PHP verwenden möchten, sollten Sie die Informationen unter [Gewusst wie: Verwenden von Notification Hubs über Java oder PHP] lesen. Sie können Benachrichtigungen von beliebigen Back-Ends mithilfe der [Notification Hubs-REST-Schnittstelle] senden.

Überspringen Sie die Schritte 1 bis 3, wenn Sie im Rahmen des Tutorials [Erste Schritte mit Notification Hubs] eine Konsolen-App für das Senden von Benachrichtigungen erstellt haben.

1. Erstellen Sie in Visual Studio eine neue Visual C#-Konsolenanwendung:
   
      ![Link „Konsolenanwendung“][13]

2. Wählen Sie im Hauptmenü von Visual Studio die Optionen **Extras** > **Bibliothekspaket-Manager** > **Paket-Manager-Konsole**, und geben Sie im Konsolenfenster die folgende Zeichenfolge ein:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
3. Drücken Sie die **EINGABETASTE**.  
    Dadurch wird mithilfe des [Microsoft.Azure.NotificationHubs-NuGet-Pakets] ein Verweis auf das Azure Notification Hubs-SDK hinzugefügt.

4. Öffnen Sie die Datei „Program.cs“, und fügen Sie die folgende `using`-Anweisung hinzu:
   
        using Microsoft.Azure.NotificationHubs;

5. Fügen Sie in der `Program` -Klasse die folgende Methode hinzu, oder ersetzen Sie sie, falls sie bereits vorhanden ist:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Send the notification as a template notification. All template registrations that contain
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

5. Ersetzen Sie im obigen Code die Platzhalter `<hub name>` und `<connection string with full access>` durch den Namen Ihres Notification Hubs und die Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* aus dem Dashboard für Ihren Notification Hub.

6. Fügen Sie in der **Main**-Methode die folgenden Zeilen hinzu:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();

7. Erstellen Sie die Konsolenanwendung.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Erste Schritte mit Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Notification Hubs-REST-Schnittstelle]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Hinzufügen von Pushbenachrichtigungen zu Mobile Apps]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[Gewusst wie: Verwenden von Notification Hubs über Java oder PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[Microsoft.Azure.NotificationHubs-NuGet-Pakets]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
