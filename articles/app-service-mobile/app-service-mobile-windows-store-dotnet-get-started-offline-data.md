<properties
	pageTitle="Aktivieren der Offlinesynchronisierung bei einer App für die universelle Windows-Plattform (UWP) mit Mobile Apps | Azure App Service"
	description="Erfahren Sie, wie Sie mit einer Azure Mobile App Offlinedaten in Ihrer App für die universelle Windows-Plattform (UWP) zwischenspeichern und synchronisieren."
	documentationCenter="windows"
	authors="wesmc7777"
	manager="dwrede"
	editor=""
	services="app-service\mobile"/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-windows"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="08/19/2016"
	ms.author="wesmc"/>

# Aktivieren der Offlinesynchronisierung für Ihre Windows-App

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## Übersicht

In diesem Tutorial wird das Hinzufügen der Offlineunterstützung zu einer App für die universelle Windows-Plattform (UWP) mithilfe eines Azure Mobile App-Back-Ends erläutert. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem Remote-Back-End synchronisiert.

In diesem Tutorial aktualisieren Sie das UWP-App-Projekt aus dem Tutorial [Erstellen einer Windows-App], damit die Offlinefunktionen von Azure Mobile Apps unterstützt werden. Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt die Datenzugriffs-Erweiterungspakete hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

Weitere Informationen über die Funktion der Offlineynchronisierung finden Sie im Thema [Offlinedatensynchronisierung in Azure Mobile Apps].

## Anforderungen

Für dieses Lernprogramm ist Folgendes erforderlich:

* Visual Studio 2013 für Windows 8.1.
* Durchführung des Lernprogramms [Erstellen einer Windows-App][create a windows app].
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite for Universal Windows Platform Development](http://www.sqlite.org/downloads) (SQLite für die Entwicklung für die universelle Windows-Plattform).

## Aktualisieren der Client-App für die Unterstützung von Offlinefunktionen

Offlinefunktionen der mobilen Azure-App ermöglichen Ihnen die Interaktion mit einer lokalen Datenbank, wenn Sie sich in einem Offlineszenario befinden. Um diese Funktionen in der App zu verwenden, initialisieren Sie einen [SyncContext][synccontext] in einem lokalen Speicher. Erstellen Sie dann für die Tabelle einen Verweis über die Schnittstelle [IMobileServiceSyncTable][IMobileServiceSyncTable]. Als lokaler Speicher auf dem Gerät wird SQLite verwendet.

1. Installieren Sie die [SQLite-Laufzeit für die Universelle Windows-Plattform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Öffnen Sie in Visual Studio den NuGet-Paket-Manager für das UWP-App-Projekt, das Sie im Tutorial [Erstellen einer Windows-App] abgeschlossen haben. Suchen und installieren Sie anschließend das NuGet-Paket **Microsoft.Azure.Mobile.Client.SQLiteStore**.

4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise** > **Verweis hinzufügen...** > **Universal Windows** > **Erweiterungen**, und aktivieren Sie dann sowohl **SQLite for Universal Windows Platform** (SQLite für die universelle Windows-Platform) als auch **Visual C++ 2015 Runtime for Universal Windows Platform apps** (Visual C++ 2015-Laufzeit für Apps der universellen Windows-Plattform).

    ![Referenz zum Hinzufügen von SQLite für UWP][1]

5. Öffnen Sie die Datei „MainPage.xaml.cs“, und heben Sie am Anfang der Datei die Auskommentierung für die folgenden `using`-Anweisungen auf:

        using Microsoft.WindowsAzure.MobileServices.SQLiteStore;  
        using Microsoft.WindowsAzure.MobileServices.Sync;         

6. Kommentieren Sie die Codezeile aus, die `todoTable` als **IMobileServiceTable** initialisiert, und heben Sie dann die Auskommentierung für die Codezeile auf, die `todoTable` als **IMobileServiceSyncTable** initialisiert:

        //private IMobileServiceTable<TodoItem> todoTable = App.MobileService.GetTable<TodoItem>();
        private IMobileServiceSyncTable<TodoItem> todoTable = App.MobileService.GetSyncTable<TodoItem>(); 

7. Heben Sie im Bereich `Offline sync` von „MainPage.xaml.cs“ die Auskommentierung der folgenden Methoden auf:

        private async Task InitLocalStoreAsync()
        {
            if (!App.MobileService.SyncContext.IsInitialized)
            {
                var store = new MobileServiceSQLiteStore("localstore.db");
                store.DefineTable<TodoItem>();
                await App.MobileService.SyncContext.InitializeAsync(store);
            }

            await SyncAsync();
        }

        private async Task SyncAsync()
        {
            await App.MobileService.SyncContext.PushAsync();
            await todoTable.PullAsync("todoItems", todoTable.CreateQuery());
        }

	In `SyncAsync` wird über [SyncContext][synccontext] ein Pushvorgang gestartet, auf den eine inkrementelle Synchronisierung folgt. Der Synchronisierungskontext verfolgt die vom Client an allen Tabellen vorgenommenen Änderungen. Weitere Informationen zu diesem Verhalten finden Sie unter [Offlinedatensynchronisierung in Azure Mobile Apps].

8. Heben Sie im `OnNavigatedTo`-Ereignishandler die Auskommentierung des Aufrufs von `InitLocalStoreAsync` auf. Wenn Sie bereits das [Tutorial zur Authentifizierung](app-service-mobile-windows-store-dotnet-get-started-users.md) abgeschlossen haben, muss dieser Schritt in der `AuthenticateAsync`-Methode ausgeführt werden.

9. Heben Sie die Auskommentierung der Aufrufe von [PushAsync] in den Methoden `InsertTodoItem` und `UpdateCheckedTodoItem` auf, und heben Sie dann die Auskommentierung des Aufrufs von `SyncAsync` in der `ButtonRefresh_Click`-Methode auf.

10. Fügen Sie in der `SyncAsync`-Methode die folgenden Ausnahmehandler hinzu:

        private async Task SyncAsync()
        {
            String errorString = null;

            try
            {
                await App.MobileService.SyncContext.PushAsync();
                await todoTable.PullAsync("todoItems", todoTable.CreateQuery()); 
            }

            catch (MobileServicePushFailedException ex)
            {
                errorString = "Push failed because of sync errors. You may be offine.\nMessage: " +
                  ex.Message + "\nPushResult.Status: " + ex.PushResult.Status.ToString();
            }
            catch (Exception ex)
            {
                errorString = "Pull failed: " + ex.Message +
                  "\n\nIf you are still in an offline scenario, " +
                  "you can try your pull again when connected with your backend.";
            }

            if (errorString != null)
            {
                MessageDialog d = new MessageDialog(errorString);
                await d.ShowAsync();
            }
        }

	In einer Offlinesituation kann [PullAsync][PullAsync] zu einer [MobileServicePushFailedException][MobileServicePushFailedException] mit folgendem [PushResult.Status][Status] führen: [CancelledByNetworkError][CancelledByNetworkError]. Dies tritt ein, wenn ein impliziter Push versucht, ausstehende Updates vor dem Pullabruf per Push zu übertragen. Es tritt dann ein Fehler auf. Weitere Informationen finden Sie unter [Synchronisierung von Offlinedaten in Azure Mobile Apps].

11. Drücken Sie in Visual Studio die Taste **F5**, um die Client-App erneut zu erstellen und auszuführen. Die App funktioniert genauso wie vor dem Aktivieren der Offlinesynchronisierung. Allerdings wird die lokale Datenbank jetzt mit Daten aufgefüllt, die in einem Offlineszenario verwendet werden können. Als Nächstes erstellen Sie ein Offlineszenario und verwenden die lokal gespeicherten Daten zum Starten der App.

## <a name="update-sync"></a>Aktualisieren der App zur Trennung der Verbindung mit dem Back-End

In diesem Abschnitt trennen Sie die Verbindung mit Ihrem Mobile App-Back-End, um ein Offlineszenario zu simulieren. Wenn Sie Datenelemente hinzufügen, teilt Ihnen der Ausnahmehandler mit, dass sich die App im Offlinemodus befindet. In diesem Zustand werden neue Elemente im lokalen Speicher hinzugefügt. Sie werden mit dem Back-End für die mobile App synchronisiert, sobald nach dem Wiederherstellen der Verbindung der nächste Push ausgeführt wird.

1. Bearbeiten Sie "App.xaml.cs" im freigegebenen Projekt. Kommentieren Sie die Initialisierung von **MobileServiceClient** aus, und fügen Sie die folgenden Zeilen hinzu, die eine ungültige mobile App-URL verwenden:

         public static MobileServiceClient MobileService =
				new MobileServiceClient("https://your-service.azurewebsites.fail");

	Beachten Sie, dass bei der Anmeldung ein Fehler auftritt, wenn in Ihrer App auch Authentifizierung verwendet wird. Sie können auch Offlineverhalten demonstrieren, indem Sie WLAN und Mobilfunknetze auf dem Gerät deaktivieren oder den Flugzeugmodus verwenden.

2. Drücken Sie **F5**, um die Anwendung zu erstellen und auszuführen. Beachten Sie, dass die Synchronisierung bei der Aktualisierung fehlgeschlagen ist, als die App gestartet wurde.
 
3. Geben Sie neue Elemente ein. Beachten Sie, dass der Pushvorgang bei jedem Klick auf **Speichern** mit dem Status [CancelledByNetworkError] fehlschlägt. Die neuen Todo-Elemente sind jedoch im lokalen Speicher vorhanden und bleiben dort, bis sie per Push zum Back-End für die mobile App übertragen werden können.

	Wenn Sie in einer Produktions-App diese Ausnahmen unterdrücken, verhält sich die Client-App, als ob sie immer noch mit dem Back-End für die mobile App verbunden wäre.

4. Schließen Sie die App, und starten Sie sie neu, um zu überprüfen, ob die neuen Elemente dauerhaft im lokalen Speicher gespeichert wurden.

5. (Optional:) Öffnen Sie den **Server-Explorer** in Visual Studio. Navigieren Sie zu der Datenbank in **Azure** -> **SQL-Datenbanken**. Klicken Sie mit der rechten Maustaste auf Ihre Datenbank, und wählen Sie **In SQL Server-Objekt-Explorer öffnen** aus. Jetzt können nach Ihrer SQL-Datenbanktabelle und seinen Inhalten suchen. Stellen Sie sicher, dass die Daten in der Back-End-Datenbank nicht geändert wurden.

6. (Optional) Verwenden Sie ein REST-Tool wie Fiddler oder Postman, um Ihr mobiles Back-End mit einer GET-Abfrage in Form von `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem` abzufragen.

## <a name="update-online-app"></a>Aktualisieren der App zum erneuten Verbinden des mobilen App-Back-Ends

In diesem Abschnitt verbinden Sie die App erneut mit dem mobilen App-Back-End. Dies simuliert, dass die App von einem Offlinestatus in einen Onlinestatus mit dem mobilen App-Back-End wechselt. Beim ersten Ausführen der Anwendung ruft der `OnNavigatedTo`-Ereignishandler `InitLocalStoreAsync` auf. Dadurch wird wiederum `SyncAsync` aufgerufen, um den lokalen Speicher mit der Back-End-Datenbank zu synchronisieren. Daher versucht die App, die Synchronisierung beim Start auszuführen.

1. Öffnen Sie „App.xaml.cs“ im freigegebenen Projekt, und heben Sie die Auskommentierung der vorherigen Initialisierung von `MobileServiceClient` auf, um die korrekte URL für die mobile App zu verwenden.

2. Drücken Sie die Taste **F5**, um die App erneut zu erstellen und auszuführen. Die App synchronisiert die lokalen Änderungen mit dem Azure Mobile App-Back-End mithilfe von Push- und Pullvorgängen, sobald der `OnNavigatedTo`-Ereignishandler ausgeführt wird.

3. (Optional) Zeigen Sie die aktualisierten Daten mithilfe von SQL Server-Objekt-Explorer oder einem REST-Tool wie Fiddler an. Beachten Sie, dass die Daten zwischen der Azure Mobile App-Back-End-Datenbank und dem lokalen Speicher synchronisiert wurden.

4. Klicken Sie in der App auf das Kontrollkästchen neben einigen Elementen, um sie im lokalen Speicher abzuschließen.

  `UpdateCheckedTodoItem` ruft `SyncAsync` auf, um jedes abgeschlossene Element mit dem Mobile App-Back-End zu synchronisieren. `SyncAsync` ruft sowohl Push- als auch Pullvorgänge auf. Sie sollten jedoch beachten, dass **bei jeder Ausführung eines Pullvorgangs gegen eine Tabelle, die der Client geändert hat, zunächst immer automatisch ein Pushvorgang im Clientsynchronisierungskontext ausgeführt wird**. Dadurch wird sichergestellt, dass alle Tabellen im lokalen Speicher und die Beziehungen konsistent bleiben. In diesem Fall hätte der Aufruf von `PushAsync` entfernt werden können, da er automatisch bei der Ausführung eines Pullvorgangs ausgeführt wird. Dieses Verhalten kann zu einem unerwarteten Pushvorgang führen, wenn Sie sich dessen nicht bewusst sind. Weitere Informationen zu diesem Verhalten finden Sie unter [Offlinedatensynchronisierung in Azure Mobile Apps].


##Zusammenfassung für die API

Zur Unterstützung der Offlinefunktionen mobiler Dienste haben Sie die Schnittstelle [IMobileServiceSyncTable] verwendet und [MobileServiceClient.SyncContext][synccontext] mit einer lokalen SQLite-Datenbank initialisiert. In einer Offlinesituation funktionieren die normalen CRUD-Operationen für Mobile Apps so, als ob die App immer noch verbunden wäre. Alle Operationen werden jedoch nur im lokalen Speicher ausgeführt. Zur Synchronisierung des lokalen Speichers mit dem Server werden die folgenden Methoden verwendet:

*  **[PushAsync]**  
   Da diese Methode [IMobileServicesSyncContext] angehört, werden Änderungen an allen Tabellen per Pushvorgang an das Back-End übertragen. Nur Datensätze mit lokalen Änderungen werden an den Server gesendet.

* **[PullAsync]**   
   Ein Pullvorgang wird von einer [IMobileServiceSyncTable] aus gestartet. Wenn verfolgte Änderungen in der Tabelle vorhanden sind, wird ein impliziter Pushvorgang ausgeführt, um sicherzustellen, dass alle Tabellen im lokalen Speicher gemeinsam mit den Beziehungen konsistent bleiben. Der *pushOtherTables*-Parameter steuert, ob bei einem impliziten Pushvorgang auch andere Tabellen per Push übertragen werden. Der *query*-Parameter akzeptiert eine [IMobileServiceTableQuery&lt;U&gt;][IMobileServiceTableQuery] oder eine OData-Abfragezeichenfolge, um die zurückgegebenen Daten zu filtern. Der *queryId*-Parameter dient zum Definieren der inkrementellen Synchronisierung. Weitere Informationen finden Sie unter [Synchronisierung von Offlinedaten in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]**  
   Ihre App sollte diese Methode in regelmäßigen Abständen aufrufen, um veraltete Daten aus dem lokalen Speicher zu löschen. Verwenden Sie den *force*-Parameter, wenn Sie noch nicht synchronisierte Änderungen löschen müssen.

Weitere Informationen zu diesen Konzepten finden Sie unter [Synchronisierung von Offlinedaten in Azure Mobile Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## Weitere Informationen

In den folgenden Themen finden Sie weitere Hintergrundinformationen zum Offlinesynchronisierungsfeature von Mobile Apps:

* [Offlinedatensynchronisierung in Azure Mobile Apps]
* [Cloud Cover: Offlinesynchronisierung in Azure Mobile Services] \(Hinweis: Im Video geht es zwar um Mobile Services, aber die Offlinesynchronisierung in Azure Mobile Apps funktioniert auf ähnliche Weise.)
* [Azure Friday: Offlinefähige Apps in Azure Mobile Services]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]: #next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offlinedatensynchronisierung in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[Synchronisierung von Offlinedaten in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[create a windows app]: app-service-mobile-windows-store-dotnet-get-started.md
[Erstellen einer Windows-App]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[Cloud Cover: Offlinesynchronisierung in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offlinefähige Apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

<!---HONumber=AcomDC_0824_2016-->