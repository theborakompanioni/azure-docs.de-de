<properties
    pageTitle="Aktivieren der Offlinesynchronisierung für Ihre Azure Mobile App (Xamarin.Forms) | Microsoft Azure"
    description="Erfahren Sie, wie Sie mobile App Service-Apps verwenden, um Offlinedaten in Ihrer Xamarin.Forms-Anwendung zwischenzuspeichern und zu synchronisieren."
    documentationCenter="xamarin"
    authors="ggailey777"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
	ms.date="06/18/2016"
    ms.author="glenga"/>

# Aktivieren der Offlinesynchronisierung für Ihre mobile Xamarin.Forms-App

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## Übersicht
In diesem Tutorial wird die Funktion zur Offlinesynchronisierung von Azure Mobile Apps für Xamarin.Forms eingeführt. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem Remotedienst synchronisiert.

Dieses Tutorial basiert auf der Xamarin.Forms-Schnellstartlösung für Mobile Apps, die Sie beim Durcharbeiten des Tutorials [Erstellen einer Xamarin iOS-App] erstellen. Die Schnellstartlösung für Xamarin.Forms enthält den Code zur Unterstützung der Offlinesynchronisierung, die lediglich aktiviert werden muss. In diesem Tutorial aktualisieren Sie die Schnellstartlösung, um die Offlinefunktionen von Azure Mobile Apps zu aktivieren. Außerdem gehen wir speziell auf den offlinespezifischen Code in der App ein. Wenn Sie die heruntergeladene Schnellstartlösung nicht verwenden, müssen Sie Ihrem Projekt die Datenzugriffs-Erweiterungspakete hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

Weitere Informationen zur Offlinesynchronisierungsfunktion finden Sie im Thema [Offlinedatensynchronisierung in Azure Mobile Apps].

## Aktivieren der Funktionen für die Offlinesynchronisierung in der Schnellstartlösung

Der Code für die Offlinesynchronisierung wird mit C#-Präprozessordirektiven in das Projekt eingefügt. Wenn das **OFFLINE\_SYNC\_ENABLED**-Symbol definiert wird, werden diese Codepfade in den Build eingefügt. Für Windows-Apps müssen Sie auch die SQLite-Plattform installieren.

1. Klicken Sie in Visual Studio mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete verwalten für Projektmappe...**. Suchen Sie anschließend nach dem NuGet-Paket **Microsoft.Azure.Mobile.Client.SQLiteStore**, und installieren Sie es für alle Projekte der Projektmappe.

2. Öffnen Sie im Projektmappen-Explorer die Datei „TodoItemManager.cs“ aus dem Projekt mit **Portable** im Namen, also das Projekt „Portable Klassenbibliothek“. Heben Sie anschließend die Auskommentierung der folgenden Präprozessordirektive auf:

		#define OFFLINE_SYNC_ENABLED

3. Öffnen Sie im Projektmappen-Explorer die Datei „TodoList.xaml.cs“ im Projekt **Portable**, und suchen Sie nach der **OnAppearing**-Methode. Stellen Sie wie folgt sicher, dass `true` für *syncItems* übergeben wird, wenn **RefreshItems** aufgerufen wird:

		await RefreshItems(true, syncItems: true);
	Dies bedeutet, dass für die App nach dem Starten versucht wird, eine Synchronisierung mit dem Back-End durchzuführen.
 
4. (Optional) Wenn Sie iOS-Geräte unterstützen, können Sie die Datei „AppDelegate.cs“ aus dem Projekt **iOS** öffnen und die Auskommentierung der folgenden Codezeile in der **FinishedLaunching**-Methode aufheben:

		SQLitePCL.CurrentPlatform.Init();

	Der SQLite Store wird hiermit auf iOS-Geräten initialisiert. Die restlichen Aufgaben sind nur zur Unterstützung von Windows-Geräten erforderlich.

5. (Optional) Um Windows-Geräte zu unterstützen, können Sie eines der folgenden SQLite-Laufzeitpakete installieren:

	* **Windows 8.1-Runtime:** Installieren Sie [SQLite für Windows 8.1](http://go.microsoft.com/fwlink/p/?LinkID=716919).
    * **Windows Phone 8.1:** Installieren Sie [SQLite für Windows Phone 8.1](http://go.microsoft.com/fwlink/p/?LinkID=716920).
    * **Universelle Windows-Plattform** Installieren Sie [SQLite für die universelle Windows-Plattform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix). Der Schnellstart enthält derzeit kein UWP-Projekt (Universelle Windows-Plattform).

6. (Optional) Klicken Sie in jedem Windows-App-Projekt mit der rechten Maustaste auf **Verweise** > **Verweis hinzufügen...**, erweitern Sie den Ordner **Windows**, und wählen Sie **Erweiterungen**. Aktivieren Sie anschließend das richtige SDK **SQLite für Windows Runtime** zusammen mit dem SDK **Visual C++ 2013 Runtime for Windows**. Beachten Sie, dass sich die Namen der SQLite-SDKs für die einzelnen Windows-Plattformen leicht unterscheiden. Installieren Sie für ein UWP-Projekt das SDK **Visual C++ 2015 Runtime for Universal Windows Platform apps**.


## Überprüfen des Clientcodes für die Synchronisierung

Hier erhalten Sie eine kurze Übersicht darüber, was bereits im Code des Tutorials in den `#if OFFLINE_SYNC_ENABLED`-Direktiven enthalten ist. Beachten Sie, dass sich die Funktionen der Offlinesynchronisierung in der Projektdatei „TodoItemManager.cs“ im Projekt „Portable Klassenbibliothek“ befinden. Eine grundlegende Übersicht über die Funktion finden Sie unter [Offlinedatensynchronisierung in Azure Mobile Apps].

* Bevor Tabellenvorgänge durchgeführt werden können, muss der lokale Speicher initialisiert werden. Die lokale Speicherdatenbank wird im **TodoItemManager**-Klassenkonstruktor mit dem folgenden Code initialisiert:

	    var store = new MobileServiceSQLiteStore("localstore.db");
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

	Er erstellt eine neue lokale SQLite-Datenbank mit der **MobileServiceSQLiteStore**-Klasse. Die **DefineTable**-Methode erstellt eine Tabelle im lokalen Speicher, die mit den Feldern im bereitgestellten Typ übereinstimmt (in diesem Fall `ToDoItem`). Der Typ muss nicht alle Spalten der Remotedatenbank enthalten. Es ist möglich, nur eine Teilmenge der Spalten zu speichern.

* Das Feld **todoTable** in **TodoItemManager** hat den Typ **IMobileServiceSyncTable** und nicht den Typ **IMobileServiceTable**. Diese Klasse verwendet die lokale Datenbank für alle Tabellenvorgänge vom Typ Erstellen, Lesen, Aktualisieren und Löschen (CRUD). Sie entscheiden, wann diese Änderungen auf das mobile App-Back-End übertragen werden, indem Sie **PushAsync** im **IMobileServiceSyncContext** aufrufen, der vom Client verwendet wird. Dieser Synchronisierungskontext hilft dabei, Tabellenbeziehungen durch das Nachverfolgen und Übertragen von Änderungen in allen Tabellen beizubehalten, die eine Clientanwendung geändert hat, wenn **PushAsync** aufgerufen wird.

	Die folgende **SyncAsync**-Methode wird für die Synchronisierung mit dem mobilen App-Back-End aufgerufen:

		public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling. 
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
						error.TableName, error.Item["id"]);
                }
            }
        }

	In diesem Beispiel wird die einfache Fehlerbehandlung mit dem Standardhandler für die Synchronisierung verwendet. In einer echten Anwendung werden die verschiedenen Fehler, z.B. Netzwerkbedingungen, Serverkonflikte und andere, mit einer benutzerdefinierten **IMobileServiceSyncHandler**-Implementierung behandelt.

##Aspekte der Offlinesynchronisierung

Im Beispiel wird die **SyncAsync**-Methode nur beim Starten und bei einer speziellen Anforderung einer Synchronisierung aufgerufen. Öffnen Sie zum Initiieren der Synchronisierung in einer Android- oder iOS-App die Elementliste. Verwenden Sie für Windows die Schaltfläche **Synchronisierung**. In einer echten Anwendung können Sie dies auch so einrichten, dass diese Synchronisierungsfunktion ausgelöst wird, wenn sich der Netzwerkstatus ändert.

Bei einem Pullvorgang für eine Tabelle mit ausstehenden lokalen Updates, die durch den Kontext verfolgt werden, löst dieser Pullvorgang automatisch einen vorherigen Kontextpush aus. Beim Aktualisieren, Hinzufügen und Abschließen von Elementen in diesem Beispiel können Sie den expliziten **PushAsync**-Aufruf weglassen, da er redundant ist.

Im bereitgestellten Code werden alle Datensätze in der TodoItem-Remotetabelle abgefragt, es ist aber auch möglich, Datensätze durch Übergeben einer Abfrage-ID und Abfrage an **PushAsync** zu filtern. Weitere Informationen finden Sie im Abschnitt *Inkrementelle Synchronisierung* in [Offlinedatensynchronisierung in Azure Mobile Apps].


## Ausführen der Client-App

Da die Offlinesynchronisierung jetzt synchronisiert wurde, können Sie die Clientanwendung nun mindestens einmal auf jeder Plattform ausführen, um die lokale Speicherdatenbank aufzufüllen. Weiter unten simulieren Sie ein Offlineszenario und ändern die Daten im lokalen Speicher, während die App offline ist.

## Aktualisieren des Synchronisierungsverhaltens der Client-App

In diesem Abschnitt ändern Sie das Clientprojekt, um ein Offlineszenario mithilfe einer ungültigen Anwendungs-URL für Ihr Back-End zu simulieren. Beim Hinzufügen oder Ändern von Datenelementen werden diese Änderungen im lokalen Speicher gespeichert, aber nicht mit dem Back-End-Datenspeicher synchronisiert, bis die Verbindung wiederhergestellt ist.

2. Öffnen Sie im Projektmappen-Explorer die Projektdatei „Constants.cs“ des Projekts **Portable**, und ändern Sie den Wert von `ApplicationURL` so, dass er auf eine ungültige URL zeigt, z.B.:

        publis static string ApplicationURL = @"https://your-service.azurewebsites.xxx/";


2. Öffnen Sie die Datei „TodoItemManager.cs“ im Projekt **Portable**, und fügen Sie dem **try...catch**-Block in **SyncAsync** dann ein weiteres **catch**-Element für die **Exception**-Basisklasse hinzu. Mit diesem **catch**-Block wird die Ausnahmemeldung wie folgt in die Konsole geschrieben:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Erstellen Sie die Client-App, und führen Sie sie aus. Fügen Sie einige neue Elemente hinzu, und beachten Sie, dass in der Konsole für jede versuchte Synchronisierung mit dem Back-End eine Ausnahme protokolliert wird. Diese neuen Elemente sind nur im lokalen Speicher vorhanden, bis sie per Push an das mobile Back-End übertragen werden können. Die Client-App verhält sich so, als ob eine Verbindung mit dem mobilen Back-End vorhanden wäre, und unterstützt alle Operationen zum Erstellen, Lesen, Aktualisieren und Löschen (CRUD-Vorgänge).

4. Schließen Sie die App, und starten Sie sie neu, um zu überprüfen, ob die neuen Elemente dauerhaft im lokalen Speicher gespeichert wurden.

5. (Optional) Verwenden Sie Visual Studio zum Anzeigen der Azure SQL-Datenbanktabelle, um festzustellen, ob sich die Daten in der Back-End-Datenbank nicht geändert haben.

	Öffnen Sie den **Server-Explorer** in Visual Studio. Navigieren Sie zu der Datenbank in **Azure** -> **SQL-Datenbanken**. Klicken Sie mit der rechten Maustaste auf Ihre Datenbank, und wählen Sie **In SQL Server-Objekt-Explorer öffnen** aus. Jetzt können nach Ihrer SQL-Datenbanktabelle und seinen Inhalten suchen.


## Aktualisieren der Client-App, um erneut eine Verbindung mit dem mobilen Back-End herzustellen

In diesem Abschnitt stellen Sie erneut eine Verbindung zwischen der App und dem mobilen Back-End her, um zu simulieren, dass die App wieder im Onlinezustand ist. Wenn Sie die Aktualisierungsbewegung durchführen, werden Daten mit Ihrem mobilen Back-End synchronisiert.

1. Öffnen Sie erneut die Datei „Constants.cs“, und korrigieren Sie dann die `applicationURL`, damit auf die richtige URL verwiesen wird.

2. Erstellen Sie die Client-App erneut, und führen Sie sie aus. Die App versucht, sich nach dem Start mit dem mobilen App-Back-End zu synchronisieren. Vergewissern Sie sich, dass in der Debugkonsole keine Ausnahmen protokolliert werden.

3. (Optional) Zeigen Sie die aktualisierten Daten mithilfe von SQL Server-Objekt-Explorer oder einem REST-Tool wie Fiddler an. Beachten Sie, dass die Daten zwischen der Back-End-Datenbank und dem lokalen Speicher synchronisiert wurden.

    Beachten Sie, dass die Daten zwischen der Datenbank und dem lokalen Speicher synchronisiert wurden und die Elemente enthalten, die Sie hinzugefügt haben, während Ihre App von der Verbindung getrennt war.

## Zusätzliche Ressourcen

* [Offlinedatensynchronisierung in Azure Mobile Apps]

* [Cloud Cover: Offlinesynchronisierung in Azure Mobile Services] \(Hinweis: Im Video geht es zwar um Mobile Services, aber die Offlinesynchronisierung in Azure Mobile Apps funktioniert auf ähnliche Weise.)

<!-- ##Summary

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## Next steps

* [Handling conflicts with offline support for Mobile Services]

* [How to use the Xamarin Component client for Azure Mobile Services]
 -->

<!-- Images -->

<!-- URLs. -->
[Erstellen einer Xamarin iOS-App]: app-service-mobile-xamarin-ios-get-started.md
[Offlinedatensynchronisierung in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md
[How to use the Xamarin Component client for Azure Mobile Services]: partner-xamarin-mobile-services-how-to-use-client-library.md
[Cloud Cover: Offlinesynchronisierung in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri

<!---HONumber=AcomDC_0629_2016-->