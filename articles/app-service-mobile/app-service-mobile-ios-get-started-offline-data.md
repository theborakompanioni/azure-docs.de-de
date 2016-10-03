<properties
	pageTitle="Aktivieren der Offlinesynchronisierung für Ihre mobile Azure-App (iOS)"
	description="Erfahren Sie, wie Sie mobile App Service-Apps verwenden, um Offlinedaten in Ihrer iOS-Anwendung zwischenzuspeichern und zu synchronisieren."
	documentationCenter="ios"
	authors="krisragh"
	manager="dwrede"
	editor=""
	services="app-service\mobile"/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="08/01/2016"
	ms.author="krisragh"/>

# Aktivieren der Offlinesynchronisierung für Ihre mobile iOS-App

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## Übersicht

In diesem Lernprogramm wird die Offlinesynchronisierungsfunktion von Azure Mobile Apps für iOS behandelt. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem Remote-Back-End synchronisiert.

Falls Sie noch keine Erfahrung mit Azure Mobile Apps haben, sollten Sie zunächst das Lernprogramm [Erstellen einer iOS-App] abschließen. Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt die Datenzugriffs-Erweiterungspakete hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

Weitere Informationen zur Offlinesynchronisierungsfunktion finden Sie im Thema [Offlinedatensynchronisierung in Azure Mobile Apps].

## <a name="review-sync"></a>Überprüfen des Clientcodes für die Synchronisierung

Das Clientprojekt, das Sie für das Lernprogramm [Erstellen einer iOS-App] heruntergeladen haben, enthält bereits Code zur Unterstützung der Offlinesynchronisierung mithilfe einer lokalen auf Kerndaten basierenden Datenbank. Dieser Abschnitt ist eine Zusammenfassung, was bereits im Code des Lernprogramms enthalten ist. Eine grundlegende Übersicht über die Funktion finden Sie unter [Offlinedatensynchronisierung in Azure Mobile Apps].

Mit der Funktion zur Offlinesynchronisierung von Azure Mobile Apps können Endbenutzer mit einer lokalen Datenbank interagieren, wenn das Netzwerk nicht verfügbar ist. Um diese Funktionen in Ihrer App zu verwenden, initialisieren Sie den Synchronisierungskontext von `MSClient`, und verweisen Sie auf einen lokalen Datenspeicher. Erstellen Sie dann über die `MSSyncTable`-Schnittstelle einen Verweis für die Tabelle.

1. Beachten Sie, dass in **QSTodoService.m** (Objective-C) oder **ToDoTableViewController.swift** (Swift) der Typ des Mitglieds `syncTable` `MSSyncTable` ist. Bei der Offlinesynchronisierung wird diese Synchronisierungstabellen-Schnittstelle anstelle von `MSTable` verwendet. Wenn eine Synchronisierungstabelle verwendet wird, werden alle Vorgänge im lokalen Speicher gespeichert und nur über explizite Push- und Pullvorgänge mit dem Remote-Back-End synchronisiert.

    Um einen Verweis auf eine Synchronisierungstabelle zu erstellen, wenden Sie die Methode `syncTableWithName` auf `MSClient` an. Zum Entfernen der Offlinesynchronisierung verwenden Sie stattdessen `tableWithName`.

2. Bevor Tabellenvorgänge durchgeführt werden können, muss der lokale Speicher initialisiert werden. Hier ist der entsprechende Code.
	
	**Objective-C**:
	
	In der `QSTodoService.init`-Methode:
	
	
	        MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
	        self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
	
	
	**Swift:**
	
	In der `ToDoTableViewController.viewDidLoad`-Methode:
	
	
	        let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
	        let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
	        self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
	        client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
	

	Dadurch wird ein lokaler Speicher mithilfe der `MSCoreDataStore`-Schnittstelle erstellt, die in der Mobile Apps-SDK bereitgestellt wird. Sie können stattdessen auch einen anderen lokalen Speicher bereitstellen, indem Sie das `MSSyncContextDataSource`-Protokoll implementieren.
	
	Außerdem wird der erste Parameter von `MSSyncContext` zum Angeben eines Konflikthandlers verwendet. Da wir `nil` weitergegeben haben, verwenden wir den Standardkonflikthandler, der bei jedem Konflikten einen Fehler ausgibt.
	
3. Nun führen wir die eigentliche Synchronisierung aus und rufen Daten vom Remote-Back-End ab.

	**Objective-C**:
	
	`syncData` überträgt zuerst neue Änderungen und ruft dann `pullData` zum Abrufen von Daten vom Remote-Back-End auf. Im Gegenzug erhält die Methode `pullData` die neuen Daten, die einer Abfrage entsprechen:
	
	
	        -(void)syncData:(QSCompletionBlock)completion
	        {
	            // push all changes in the sync context, then pull new data
	            [self.client.syncContext pushWithCompletion:^(NSError *error) {
	                [self logErrorIfNotNil:error];
	                [self pullData:completion];
	            }];
	        }
	
	        -(void)pullData:(QSCompletionBlock)completion
	        {
	            MSQuery *query = [self.syncTable query];
	
	            // Pulls data from the remote server into the local table.
	            // We're pulling all items and filtering in the view
	            // query ID is used for incremental sync
	            [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
	                [self logErrorIfNotNil:error];
	
	                // Let the caller know that we have finished
	                if (completion != nil) {
	                    dispatch_async(dispatch_get_main_queue(), completion);
	                }
	            }];
	        }
        
        
      **Swift**:
        
        
		func onRefresh(sender: UIRefreshControl!) {
		    UIApplication.sharedApplication().networkActivityIndicatorVisible = true
		    
		    self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
		        (error) -> Void in
		        
		        UIApplication.sharedApplication().networkActivityIndicatorVisible = false
		        
		        if error != nil {
		            // A real application would handle various errors like network conditions,
		            // server conflicts, etc via the MSSyncContextDelegate
		            print("Error: (error!.description)")
		            
		            // We will just discard our changes and keep the servers copy for simplicity
		            if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
		                for opError in opErrors {
		                    print("Attempted operation to item (opError.itemId)")
		                    if (opError.operation == .Insert || opError.operation == .Delete) {
		                        print("Insert/Delete, failed discarding changes")
		                        opError.cancelOperationAndDiscardItemWithCompletion(nil)
		                    } else {
		                        print("Update failed, reverting to server's copy")
		                        opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
		                    }
		                }
		            }
		        }
		        self.refreshControl?.endRefreshing()
		    }
		} 
	
	
	In der Objective-C-Version rufen wir in `syncData` zuerst `pushWithCompletion` vom Synchronisierungskontext ab. Diese Methode ist ein Mitglied von `MSSyncContext` (statt der Synchronisierungstabelle selbst), da sie Änderungen auf alle Tabellen übertragen wird. Nur Datensätze, die lokal geändert wurden (mit CRUD-Operationen), werden an den Server gesendet. Anschließend wird das Hilfsprogramm `pullData` aufgerufen, das wiederum `MSSyncTable.pullWithQuery` aufruft, um Remotedaten abzurufen und in der lokalen Datenbank zu speichern.
	
	In der Swift-Version erfolgt kein Aufruf von `pushWithCompletion`. Grund: Der Pushvorgang war nicht unbedingt notwendig. Wenn im Synchronisierungskontext der Tabelle, die einen Pushvorgang durchführt, Änderungen ausstehen, wird durch den Pullvorgang immer zuerst ein Pushvorgang ausgeführt. Wenn Sie über mehr als eine Synchronisierungstabelle verfügen, ist es jedoch am besten, explizit einen Pushvorgang auszuführen, um sicherzustellen, dass die verknüpften Tabellen konsistent sind.
	
	Sowohl in der Objective-C- als auch Swift-Version können Sie mit der Methode `pullWithQuery` eine Abfrage zum Filtern der Datensätze angeben, die Sie abrufen möchten. In diesem Beispiel ruft die Abfrage einfach alle Datensätze in der Remotetabelle `TodoItem` ab.
	
	Der zweite Parameter für `pullWithQuery` ist eine Abfrage-ID, die für die *inkrementelle Synchronisierung* verwendet wird. Die inkrementelle Synchronisierung ruft nur die Datensätze ab, die seit der letzten Synchronisierung geändert wurden. Dafür wird der Zeitstempel `UpdatedAt` des Datensatzes (im lokalen Speicher `updatedAt`) verwendet. Die Abfrage-ID muss eine beschreibende Zeichenfolge sein, die für jede logische Abfrage in Ihrer App eindeutig ist. Um die inkrementelle Synchronisierung zu deaktivieren, übergeben Sie als Abfrage-ID `nil`. Beachten Sie, dass dies möglicherweise ineffizient ist, da auf diese Weise bei jedem Pullvorgang alle Datensätze abgerufen werden.

5. Die Objective-C-App führt eine Synchronisierung durch, wenn wir Daten ändern oder hinzufügen, ein Benutzer die Aktualisierungsbewegung ausführt, sowie beim Starten. Die Swift-App führt eine Synchronisierung durch, wenn ein Benutzer die Aktualisierungsbewegung ausführt, sowie beim Starten.

Da die App immer dann eine Synchronisierung durchführt, sobald Daten geändert werden (Objective-C), oder bei jedem Start der App (Objective-C und Swift), setzt die App voraus, dass der Benutzer online ist. In einem anderen Abschnitt werden wir die App modifizieren, sodass Benutzer auch im Offlinemodus Daten bearbeiten können.

## <a name="review-core-data"></a>Überprüfen des Core-Datenmodells

Bei Verwendung des Offlinespeichers für die Kerndaten müssen Sie bestimmte Tabellen und Felder in Ihrem Datenmodell definieren. Die Beispiel-App enthält bereits ein Datenmodell mit dem richtigen Format. In diesem Abschnitt durchlaufen wir diese Tabellen und ihre Verwendung.

- Öffnen Sie **QSDataModel.xcdatamodeld**. Es sind vier Tabellen definiert – drei, die durch das SDK verwendet werden, und eine Tabelle für die Todo-Elemente selbst:
      * MS\_TableOperations: Zum Nachverfolgen der Elemente, die mit dem Server synchronisiert werden müssen
      * MS\_TableOperationErrors: Zum Nachverfolgen von Fehlern, die während der Offlinesynchronisierung auftreten
      * MS\_TableConfig: Zum Nachverfolgen der letzten aktualisierten Zeit für die letzte Synchronisierung für alle Pullvorgänge
      * TodoItem: Zum Speichern der TODO-Elemente. Die Systemspalten **createdAt**, **updatedAt** und **version** sind optionale Systemeigenschaften.

>[AZURE.NOTE] Das Azure Mobile Apps-SDK reserviert Spaltennamen, die mit "**``**" beginnen. Dieses Präfix sollte nur für Systemspalten verwendet werden, andernfalls werden die Spaltennamen bei Verwendung des Remote-Back-Ends geändert.

- Zur Verwendung der Funktion zur Offlinesynchronisierung müssen Sie die Systemtabellen definieren, wie unten dargestellt.

    ### Systemtabellen

    **MS\_TableOperations**

    ![][defining-core-data-tableoperations-entity]

    | Attribut | Typ |
    |----------- |   ------    |
    | id | Integer 64 |
    | itemId | String |
    | Eigenschaften | Binärdaten |
    | table | String |
    | tableKind | Integer 16 |

    <br>**MS\_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

    | Attribut | Typ |
    |----------- |   ------    |
    | id | String |
    | operationId | Integer 64 |
    | Eigenschaften | Binärdaten |
    | tableKind | Integer 16 |

    <br>**MS\_TableConfig**

    ![][defining-core-data-tableconfig-entity]

    | Attribut | Typ |
    |----------- |   ------    |
    | id | String |
    | key | String |
    | keyType | Integer 64 |
    | table | String |
    | value | String |

    ### Datentabelle

    **TodoItem**

    | Attribut | Typ | Hinweis |
    |-----------   |  ------ | -------------------------------------------------------|
    | id | Zeichenfolge, als erforderlich gekennzeichnet | Primärschlüssel im Remotespeicher |
    | complete | Boolean | Todo-Elementfeld |
    | Text | String | Todo-Elementfeld |
    | createdAt | Datum | (optional) Zuordnung zur createdAt-Systemeigenschaft |
    | updatedAt | Datum | (optional) Zuordnung zur updatedAt-Systemeigenschaft |
    | Version | String | (optional) Zum Erkennen von Konflikten, Zuordnung zu „version“ |


## <a name="setup-sync"></a>Ändern des Synchronisierungsverhaltens der App

In diesem Abschnitt ändern Sie die Anwendung, sodass beim Starten der App oder beim Einfügen und Aktualisieren von Elementen keine Synchronisierung erfolgt, sondern nur, wenn die Aktualisierungsschaltfläche betätigt wird.

**Objective-C**:

1. Ändern Sie in **QSTodoListViewController.m**, die **viewDidLoad**-Methode, um den Aufruf von `[self refresh]` am Ende der Methode zu entfernen. Die Daten werden jetzt beim Starten der App nicht mit dem Server synchronisiert, sondern stattdessen die Inhalte des lokalen Speichers.

2. Ändern Sie in **QSTodoService.m** die Definition von `addItem`, sodass nach dem Einfügen des Elements keine Synchronisierung erfolgt. Entfernen Sie den `self syncData`-Block, und ersetzen Sie ihn durch Folgendes:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Ändern Sie die Definition von `completeItem` wie oben beschrieben. Entfernen Sie den Block für `self syncData`, und ersetzen Sie ihn durch Folgendes:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. Kommentieren Sie in `viewDidLoad` in **ToDoTableViewController.swift** diese beiden Zeilen aus, um die Synchronisierung beim Starten der App zu beenden. Zum Zeitpunkt der Verfassung dieses Artikels aktualisiert die Swift-Todo-App den Dienst nicht, wenn ein Element hinzufügt oder abgeschlossen wird, sondern nur beim App-Start.

		self.refreshControl?.beginRefreshing()
		self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Testen der App

In diesem Abschnitt stellen Sie eine Verbindung mit einer ungültigen URL her, um ein Offlineszenario zu simulieren. Wenn Sie Datenelemente hinzufügen, werden diese im lokalen Kerndatenspeicher vorgehalten, jedoch nicht mit dem mobilen Back-End synchronisiert.

1. Ändern Sie die URL der mobilen App in **QSTodoService.m** zu einer ungültige URL und führen Sie sie erneut aus:

	**Objective-C** in „QSTodoService.m“:
	
        	self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
	
	**Swift** in „ToDoTableViewController.swift“:

		let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Fügen Sie einige Todo-Elemente hinzu. Beenden Sie den Simulator (oder Erzwingen Sie das Schließen der App), und starten Sie ihn neu. Stellen Sie sicher, dass die Änderungen erhalten geblieben sind.

3. Zeigen Sie den Inhalt der TodoItem-Remotetabelle an:

    + Ein Node.js-Back-End finden Sie im [Azure-Portal](https://portal.azure.com/). Dort klicken Sie in Ihrem mobilen App-Back-End auf **Easy Tables** > **TodoItem**, um den Inhalt der `TodoItem`-Tabelle anzuzeigen.
   	+ Zeigen Sie für einen .NET-Back-End den Inhalt der Tabelle entweder mit einem SQL-Tool wie SQL Server Management Studio oder einen REST-Client wie Fiddler oder Postman an.

    Stellen Sie sicher, dass die neuen Elemente *nicht* auf dem Server synchronisiert wurden:

4. Berichtigen Sie die URL nun wieder unter **QSTodoService.m** und führen Sie die Anwendung dann erneut aus. Führen Sie die Aktualisierungsbewegung aus, indem Sie die Elementliste nach unten ziehen. Ein Statusdrehfeld wird angezeigt.

5. Zeigen Sie die TodoItem-Daten erneut an. Es sollten jetzt die neuen und geänderten TodoItems angezeigt werden.

## Zusammenfassung

Um die Offlinesynchronisierungsfunktion zu unterstützen, wurde die Schnittstelle `MSSyncTable` verwendet und `MSClient.syncContext` mit einem lokalen Speicher initialisiert. In diesem Fall war der lokale Speicher eine auf Kerndaten basierende Datenbank.

Wenn Sie einen lokalen Kerndatenspeicher verwenden, müssen Sie mehrere Tabellen mit [korrekten Systemeigenschaften](#review-core-data) definieren.

Die normalen CRUD-Operationen für Azure Mobile Apps funktionieren so, als ob die App immer noch verbunden wäre. Alle Operationen werden jedoch nur im lokalen Speicher ausgeführt.

Zur Synchronisierung des lokalen Speichers mit dem Server haben Sie die Methode `MSSyncTable.pullWithQuery` verwendet.


## Zusätzliche Ressourcen

* [Offlinedatensynchronisierung in Azure Mobile Apps]

* [Cloud Cover: Offlinesynchronisierung in Azure Mobile Services] \(Hinweis: Im Video geht es zwar um Mobile Services, aber die Offlinesynchronisierung in Azure Mobile Apps funktioniert auf ähnliche Weise.)

<!-- URLs. -->


[Erstellen einer iOS-App]: ../app-service-mobile-ios-get-started.md
[Offlinedatensynchronisierung in Azure Mobile Apps]: ../app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud Cover: Offlinesynchronisierung in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

<!---HONumber=AcomDC_0810_2016-->