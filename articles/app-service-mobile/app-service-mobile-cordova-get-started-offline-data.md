---
title: "Aktivieren der Offlinesynchronisierung für Azure Mobile App (Cordova) | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Offlinedaten in Ihrer Cordova-Anwendung mithilfe einer mobilen App Service-App zwischenspeichern und synchronisieren.
documentationcenter: cordova
author: adrianhall
manager: erikre
editor: 
services: app-service\mobile
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c8f8f0188bb2b35d0d010ac55dd915519c09c159


---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Aktivieren der Offlinesynchronisierung für Ihre mobile Cordova-App
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

In diesem Tutorial wird das Offlinesynchronisierungsfeature von Azure Mobile Apps für Cordova vor vorgestellt. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (also das Anzeigen, Hinzufügen oder Ändern von Daten), auch wenn keine Netzwerkverbindung besteht. Änderungen werden in einer lokalen Datenbank gespeichert. Sobald das Gerät wieder online ist, werden diese Änderungen mit dem Remotedienst synchronisiert.

Dieses Tutorial basiert auf der Cordova-Schnellstart-Projektmappe für Mobile Apps, die Sie im Rahmen des Tutorials [Erstellen einer Apache Cordova-App]erstellen. In diesem Tutorial erweitern Sie die Schnellstart-Projektmappe mit Offlinefeatures von Azure Mobile Apps. Außerdem gehen wir speziell auf den offlinespezifischen Code in der App ein.

Weitere Informationen über die Funktion der Offlineynchronisierung finden Sie im Thema [Inkrementelle Synchronisierung]. Ausführliche Informationen zur API-Verwendung finden Sie in der [Infodatei] des Plug-Ins.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Hinzufügen der Offlinesynchronisierung zur Schnellstart-Projektmappe
Der Code für die Offlinesynchronisierung muss der App hinzugefügt werden. Für die Offlinesynchronisierung wird das Plug-In „cordova-sqlite-storage“ benötigt. Dieses wird Ihrer App automatisch hinzugefügt, wenn das Azure Mobile Apps-Plug-In in das Projekt eingeschlossen wird. Im Schnellstartprojekt sind beide Plug-Ins enthalten.

1. Öffnen Sie im Projektmappen-Explorer von Visual Studio die Projektdatei „index.js“, und ersetzen Sie den folgenden Code:
   
        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend
   
    durch diesen Code:
   
        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context
2. Ersetzen Sie als Nächstes den folgenden Code:
   
        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
   
    durch diesen Code:
   
        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
   
        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');
   
        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });
   
        // Get the sync context from the client
        syncContext = client.getSyncContext();
   
    Der obigen Codeergänzungen initialisieren den lokalen Speicher und definieren eine lokale Tabelle mit Spaltenwerten, die auch in Ihrem Back-End verwendet werden. (Sie müssen nicht alle Spaltenwerte in diesen Code einschließen.)
   
    Einen Verweis auf den Synchronisierungskontext erhalten Sie durch Aufrufen von **getSyncContext**. Der Synchronisierungskontext dient zur Beibehaltung von Tabellenbeziehungen. Hierzu werden Änderungen in allen Tabellen, die eine Clientanwendung geändert hat, nachverfolgt und übertragen, wenn **push** aufgerufen wird.
3. Legen Sie die Anwendungs-URL auf die Mobile App-Anwendungs-URL fest.
4. Ersetzen Sie als Nächstes den folgenden Code:
   
        todoItemTable = client.getTable('todoitem'); // todoitem is the table name
   
    durch diesen Code:
   
        // todoItemTable = client.getTable('todoitem');
   
        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {
   
        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');
   
        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };
   
        // Call a function to perform the actual sync
        syncBackend();
   
        // Refresh the todoItems
        refreshDisplay();
   
        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);
   
    Der obige Code initialisiert den Synchronisierungskontext und ruft dann „getSyncTable“ (anstelle von „getTable“) auf, um einen Verweis auf die lokale Tabelle abzurufen.
   
    Diese Klasse verwendet für alle Tabellenvorgänge vom Typ Erstellen, Lesen, Aktualisieren und Löschen (Create/Read/Update/Delete, CRUD) die lokale Datenbank.
   
    In diesem Beispiel wird bei Synchronisierungskonflikten eine einfache Fehlerbehandlung durchgeführt. In einer echten Anwendung werden verschiedene Fehler wie Netzwerkbedingungen, Serverkonflikte und andere Probleme behandelt. Codebeispiele finden Sie im [Beispiel zur Offlinesynchronisierung].
5. Fügen Sie im nächsten Schritt die folgende Funktion hinzu, um die eigentliche Synchronisierung durchzuführen:
   
        function syncBackend() {
   
          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed
   
          });
   
          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }
   
    Änderungen werden an das Mobile App-Back-End übertragen, wenn **push** für das vom Client verwendete **syncContext**-Objekt aufgerufen wird. So können Sie etwa einen Aufruf von **syncBackend** einem Schaltflächenereignis-Handler in der App hinzufügen, beispielsweise in Form einer neuen Synchronisierungsschaltfläche. Eine andere Möglichkeit wäre, den Aufruf zur **addItemHandler**-Funktion hinzuzufügen, sodass eine Synchronisierung stattfindet, wenn ein neues Element hinzugefügt wird.

## <a name="offline-sync-considerations"></a>Aspekte der Offlinesynchronisierung
Im vorliegenden Beispiel wird die **push**-Methode von **syncContext** nur beim Start der App in der Rückruffunktion für die Anmeldung aufgerufen.  In einer echten Anwendung kann die Synchronisierungsfunktion auch manuell oder bei einer Änderung des Netzwerkzustands ausgelöst werden.

Bei einem Pullvorgang für eine Tabelle mit ausstehenden lokalen Updates, die durch den Kontext verfolgt werden, löst dieser Pullvorgang automatisch einen vorherigen Kontextpush aus. Beim Aktualisieren, Hinzufügen und Abschließen von Elementen in diesem Beispiel können Sie den expliziten **push**-Aufruf weglassen, da er unter Umständen redundant ist. (Informieren Sie sich vorher in der [Infodatei] über den aktuellen Featurestatus.)

Im bereitgestellten Code werden alle Datensätze in der todoItem-Remotetabelle abgefragt, es ist aber auch möglich, Datensätze durch Übergeben einer Abfrage-ID und Abfrage an **push**zu filtern. Weitere Informationen finden Sie unter *Offlinedatensynchronisierung in Azure Mobile Apps* im Abschnitt [Inkrementelle Synchronisierung].

## <a name="optional-disable-authentication"></a>(Optional) Deaktivieren der Authentifizierung
Falls Sie noch keine Authentifizierung eingerichtet haben und die Offlinesynchronisierung ohne Authentifizierung testen möchten, kommentieren Sie die Rückruffunktion für die Anmeldung aus, aber lassen Sie den Code innerhalb der Rückruffunktion unkommentiert.

Nach dem Auskommentieren der Zeilen für die Anmeldung sollte der Code wie folgt aussehen:

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Die App wird nun mit dem Azure-Back-End synchronisiert, wenn Sie die App ausführen (und nicht, wenn Sie sich anmelden).

## <a name="run-the-client-app"></a>Ausführen der Client-App
Nach dem Aktivieren der Offlinesynchronisierung können Sie die Clientanwendung mindestens einmal auf jeder Plattform ausführen, um die lokale Speicherdatenbank aufzufüllen. Weiter unten simulieren Sie ein Offlineszenario und ändern die Daten im lokalen Speicher, während die App offline ist.

## <a name="optional-test-the-sync-behavior"></a>(Optional) Testen des Synchronisierungsverhaltens
In diesem Abschnitt ändern Sie das Clientprojekt, um ein Offlineszenario mithilfe einer ungültigen Anwendungs-URL für Ihr Back-End zu simulieren. Beim Hinzufügen oder Ändern von Datenelementen werden diese Änderungen im lokalen Speicher gespeichert, aber nicht mit dem Back-End-Datenspeicher synchronisiert, bis die Verbindung wiederhergestellt ist.

1. Öffnen Sie im Projektmappen-Explorer die Projektdatei „index.js“, und ändern Sie die Anwendungs-URL, sodass sie auf eine ungültige URL verweist. Beispiel:
   
        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');
2. Aktualisieren Sie in „index.html“ das `<meta>`-CSP-Element mit der gleichen ungültigen URL:
   
        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">
3. Erstellen Sie die Client-App, und führen Sie sie aus. In der Konsole wird nach der Anmeldung jedes Mal eine Ausnahme protokolliert, wenn die App versucht, eine Synchronisierung mit dem Back-End durchzuführen. Neu hinzugefügte Elemente sind nur im lokalen Speicher vorhanden, bis sie per Push an das mobile Back-End übertragen werden können. Die Client-App verhält sich so, als ob eine Verbindung mit dem mobilen Back-End vorhanden wäre, und unterstützt alle Operationen zum Erstellen, Lesen, Aktualisieren und Löschen (CRUD-Vorgänge).
4. Schließen Sie die App, und starten Sie sie neu, um zu überprüfen, ob die neuen Elemente dauerhaft im lokalen Speicher gespeichert wurden.
5. (Optional) Verwenden Sie Visual Studio zum Anzeigen der Azure SQL-Datenbanktabelle, um festzustellen, ob sich die Daten in der Back-End-Datenbank nicht geändert haben.
   
    Öffnen Sie den **Server-Explorer**in Visual Studio. Navigieren Sie in **Azure**->**SQL-Datenbanken** zu Ihrer Datenbank. Klicken Sie mit der rechten Maustaste auf Ihre Datenbank, und wählen Sie **In SQL Server-Objekt-Explorer öffnen**aus. Jetzt können nach Ihrer SQL-Datenbanktabelle und seinen Inhalten suchen.

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optional) Testen der Verbindungswiederherstellung mit Ihrem mobilen Back-End
In diesem Abschnitt stellen Sie erneut eine Verbindung zwischen der App und dem mobilen Back-End her, um zu simulieren, dass die App wieder im Onlinezustand ist. Wenn Sie sich anmelden, werden die Daten mit Ihrem mobilen Back-End synchronisiert.

1. Öffnen Sie „index.js“, und legen Sie die Anwendungs-URL wieder auf die korrekte URL fest.
2. Öffnen Sie „index.html“, und korrigieren Sie die Anwendungs-URL im `<meta>` -CSP-Element.
3. Erstellen Sie die Client-App erneut, und führen Sie sie aus. Nach der Anmeldung versucht die App, eine Synchronisierung mit dem mobilen App-Back-End durchzuführen. Vergewissern Sie sich, dass in der Debugkonsole keine Ausnahmen protokolliert werden.
4. (Optional) Zeigen Sie die aktualisierten Daten mithilfe von SQL Server-Objekt-Explorer oder einem REST-Tool wie Fiddler an. Beachten Sie, dass die Daten zwischen der Back-End-Datenbank und dem lokalen Speicher synchronisiert wurden.
   
    Beachten Sie, dass die Daten zwischen der Datenbank und dem lokalen Speicher synchronisiert wurden und die Elemente enthalten, die Sie hinzugefügt haben, während Ihre App von der Verbindung getrennt war.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Inkrementelle Synchronisierung]
* [Cloud Cover: Offlinesynchronisierung in Azure Mobile Services] \(Hinweis: Im Video geht es zwar um Mobile Services, aber die Offlinesynchronisierung in Azure Mobile Apps funktioniert auf ähnliche Weise.\)
* [Visual Studio-Tools für Apache Cordova]

## <a name="next-steps"></a>Nächste Schritte
* Machen Sie sich im [Beispiel zur Offlinesynchronisierung]
* Sehen Sie sich in der [Infodatei]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Erstellen einer Apache Cordova-App]: app-service-mobile-cordova-get-started.md
[Infodatei]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Beispiel zur Offlinesynchronisierung]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Inkrementelle Synchronisierung]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offlinesynchronisierung in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Hinzufügen von Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md



<!--HONumber=Nov16_HO3-->


