<properties
    pageTitle="Vorgehensweise beim Verwenden des iOS-SDKs für Azure Mobile Apps"
    description="Vorgehensweise beim Verwenden des iOS-SDKs für Azure Mobile Apps"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="yuaxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>


# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Verwenden der iOS-Clientbibliothek für Azure Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Dieser Artikel beschreibt gängige Szenarien für die Verwendung des aktuellen [Azure Mobile Apps-iOS-SDKs][1]. Wenn Sie mit Azure Mobile Apps noch nicht vertraut sind, führen Sie zunächst den [Schnellstart von Azure Mobile Apps] durch, um ein Back-End und eine Tabelle zu erstellen und ein vorgefertigtes iOS-Xcode-Projekt herunterzuladen. In diesem Handbuch konzentrieren wir uns auf das clientseitige iOS-SDK. Weitere Informationen zum serverseitigen SDK für das Back-End finden Sie in den Anleitungen zum jeweiligen Server-SDK.

## <a name="reference-documentation"></a>Referenzdokumentation

Die Referenzdokumentation für das iOS-Client-SDK finden Sie hier: [Azure Mobile Apps iOS-Client-Referenz][2].

## <a name="supported-platforms"></a>Unterstützte Plattformen

Das iOS-SDK unterstützt Objective-C-Projekte, Swift 2.2-Projekte und Swift 2.3-Projekte für die iOS-Versionen 8.0 oder höher.

Die Authentifizierung für den „Serverfluss“ verwendet eine Webansicht für die dargestellte Benutzeroberfläche.  Wenn das Gerät keine Benutzeroberfläche in Form einer Webansicht darstellen kann, ist eine andere Authentifizierungsmethode erforderlich, die außerhalb des Bereichs des Produkts liegt.  
Dieses SDK eignet sich daher nicht für Geräte vom Typ „Überwachung“ oder für ähnlich eingeschränkte Geräte.

##<a name="<a-name="setup"></a>setup-and-prerequisites"></a><a name="Setup"></a>Einrichtung und Voraussetzungen

Dieses Lernprogramm setzt voraus, dass Sie ein Back-End mit einer Tabelle erstellt haben. In dieser Anleitung wird davon ausgegangen, dass die Tabelle das gleiche Schema wie die Tabellen in diesen Lernprogrammen aufweist. Dieses Handbuch setzt außerdem voraus, dass Sie in Ihrem Code auf das `MicrosoftAzureMobile.framework` verweisen und `MicrosoftAzureMobile/MicrosoftAzureMobile.h` importieren.

##<a name="<a-name="create-client"></a>how-to:-create-client"></a><a name="create-client"></a>Vorgehensweise beim Erstellen von Clients

Erstellen Sie für den Zugriff auf ein Azure Mobile Apps-Back-End in Ihrem Projekt einen `MSClient`. Ersetzen Sie `AppUrl` durch die URL der App. Sie können `gatewayURLString` und `applicationKey` leer lassen. Wenn Sie ein Gateway für die Authentifizierung einrichten, befüllen Sie `gatewayURLString` mit der Gateway-URL.

**Objective-C:**

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift:**

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="<a-name="table-reference"></a>how-to:-create-table-reference"></a><a name="table-reference"></a>Gewusst wie: Erstellen von Tabellenverweisen

Zum Zugreifen auf oder Aktualisieren von Daten erstellen Sie einen Verweis auf die Back-End-Tabelle. Ersetzen Sie `TodoItem` durch den Namen Ihrer Tabelle.

**Objective-C:**

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift:**

```
let table = client.tableWithName("TodoItem")
```


##<a name="<a-name="querying"></a>how-to:-query-data"></a><a name="querying"></a>Gewusst wie: Abfragen von Daten

Fragen Sie zum Erstellen einer Abfrage das `MSTable` -Objekt ab. Die folgende Abfrage ruft alle Elemente im `TodoItem` ab und protokolliert den Text von jedem Element.

**Objective-C:**

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift:**

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="<a-name="filtering"></a>how-to:-filter-returned-data"></a><a name="filtering"></a>Gewusst wie: Filtern zurückgegebener Daten

Für das Filtern von Ergebnissen stehen zahlreiche Optionen zur Verfügung.

Um mithilfe eines Prädikats zu filtern, verwenden Sie `NSPredicate` und `readWithPredicate`. Mit den folgenden Filtern werden nur Daten zu unvollständigen Todo-Elemente zurückgegeben.

**Objective-C:**

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift:**

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="<a-name="query-object"></a>how-to:-use-msquery"></a><a name="query-object"></a>Gewusst wie: Verwenden von MSQuery

Um eine komplexe Abfrage (einschließlich Sortierung und Paging) auszuführen, erstellen Sie ein `MSQuery` -Objekt entweder direkt oder durch Verwenden eines Prädikats:

**Objective-C:**

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift:**

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery` können Sie verschiedene Abfrageverhaltensweisen steuern.

* Festlegen der Reihenfolge der Ergebnisse
* Einschränken der zurückzugebenden Felder
* Einschränken der Anzahl der zurückzugebenden Datensätze
* Festlegen der Gesamtanzahl in der Antwort
* Festlegen benutzerdefinierter Abfrageparameter in der Anforderung
* Anwenden zusätzlicher Funktionen

Führen Sie eine `MSQuery`-Abfrage aus, indem Sie `readWithCompletion` im Objekt aufrufen.

## <a name="<a-name="sorting"></a>how-to:-sort-data-with-msquery"></a><a name="sorting"></a>Gewusst wie: Sortieren von Daten mit MSQuery

Um die Ergebnisse zu sortieren, sehen wir uns ein Beispiel an. Um aufsteigend nach dem Feld „text“ und danach absteigend nach „complete“ zu sortieren, rufen Sie `MSQuery` wie folgt auf:

**Objective-C:**

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift:**

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="<a-name="selecting"></a><a-name="parameters"></a>how-to:-limit-fields-and-expand-query-string-parameters-with-msquery"></a><a name="selecting"></a><a name="parameters"></a>Gewusst wie: Einschränken von Feldern und Erweitern von Abfragezeichenfolgen-Parametern mit MSQuery

Um die in einer Abfrage zurückzugebenden Felder einzuschränken, geben Sie die Namen der Felder in der **SelectFields** -Eigenschaft an. Dieses Beispiel gibt nur die Felder „text“ und „completed“ zurück:

**Objective-C:**

```
query.selectFields = @[@"text", @"complete"];
```

**Swift:**

```
query.selectFields = ["text", "complete"]
```

Um zusätzliche Abfragezeichenfolgen-Parameter in die Anforderung einzubinden (z. B. weil diese Parameter von einem benutzerdefinierten serverseitigen Skript verwendet werden), füllen Sie `query.parameters` wie folgt aus:

**Objective-C:**

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift:**

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="<a-name="paging"></a>how-to:-configure-page-size"></a><a name="paging"></a>Gewusst wie: Konfigurieren der Seitengröße

Bei Azure Mobile Apps steuert die Seitengröße die Anzahl der Datensätze, die gleichzeitig von den Back-End-Tabellen abgerufen werden. Bei einem Aufruf von `pull`-Daten werden dann Daten basierend auf dieser Seitengröße zusammengefasst, bis keine weiteren Datensätze mehr abzurufen sind.

Eine Seitengröße kann wie unten dargestellt mit **MSPullSettings** konfiguriert werden. Die Standardseitengröße ist 50, und im folgenden Beispiel wird sie zu 3 geändert.

Eine Änderung der Seitengröße erfolgt z. B. aus Leistungsgründen. Bei einer großen Anzahl von kleinen Datensätzen verringert eine hohe Seitengröße die Anzahl der Serverroundtrips. 

Diese Einstellung steuert nur die Seitengröße auf der Clientseite. Wenn der Client eine höhere Seitengröße fordert, als das Mobile Apps-Back-End unterstützt, ist die Seitengröße auf die maximale Größe begrenzt, die vom Back-unterstützt wird. 

Diese Einstellung ist auch die _Anzahl_ von Datensätzen, nicht die _Bytegröße_.

Wenn Sie die Clientseitengröße erhöhen [sollten Sie auch die Seitengröße auf dem Server erhöhen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Objective-C:**

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift:**

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="<a-name="inserting"></a>how-to:-insert-data"></a><a name="inserting"></a>Gewusst wie: Einfügen von Daten

Um eine neue Tabellenzeile einzufügen, erstellen Sie ein `NSDictionary`, und rufen Sie `table insert` auf. Wenn das [dynamische Schema] aktiviert ist, generiert das mobile Azure App Service-Back-End automatisch neue Spalten basierend auf dem `NSDictionary`.

Wenn `id` nicht angegeben wird, generiert das Back-End automatisch eine neue eindeutige ID. Geben Sie eine eigene `id` ein, um E-Mail-Adressen, Benutzernamen oder einen eigenen benutzerdefinierten Wert als ID zu verwenden. Die Bereitstellung einer eigenen ID kann Joins und die geschäftsorientierte Datenbanklogik vereinfachen.

Das `result` enthält das neue eingefügte Element. Abhängig von Ihrer Serverlogik kann es im Vergleich zur ursprünglichen Übergabe an den Server zusätzliche oder geänderte Daten enthalten.

**Objective-C:**

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift:**

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="<a-name="modifying"></a>how-to:-modify-data"></a><a name="modifying"></a>Gewusst wie: Ändern von Daten

Um eine vorhandene Zeile zu aktualisieren, ändern Sie ein Element, und rufen Sie `update`auf:

**Objective-C:**

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift:**

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Geben Sie alternativ die Zeilen-ID und das aktualisierte Feld ein:

**Objective-C:**

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift:**

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Für Änderungen muss zumindest das `id` -Attribut gesetzt sein.

##<a name="<a-name="deleting"></a>how-to:-delete-data"></a><a name="deleting"></a>Gewusst wie: Löschen von Daten

Um ein Element zu löschen, rufen Sie `delete` mit dem Element auf:

**Objective-C:**

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift:**

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Geben Sie alternativ eine Zeilen-ID ein, um das Element zu löschen:

**Objective-C:**

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift:**

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Für Löschungen muss zumindest das `id` -Attribut gesetzt sein.

##<a name="<a-name="customapi"></a>how-to:-call-custom-api"></a><a name="customapi"></a>Gewusst wie: Aufrufen einer benutzerdefinierten API

Mit einer benutzerdefinierten API können Sie beliebige Back-End-Funktionen verfügbar machen. Sie muss keinem Tabellenvorgang zuordnet sein. Sie erhalten nicht nur mehr Kontrolle über das Messaging, sondern können sogar Header lesen/festlegen und das Layout der Antwort ändern. Informationen zum Erstellen einer benutzerdefinierten API auf dem Back-End finden Sie unter [Benutzerdefinierte APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Um eine benutzerdefinierte API aufzurufen, rufen Sie `MSClient.invokeAPI`auf. Der Inhalt der Anforderung und der Antwort wird als JSON behandelt. Zur Nutzung anderer Medientypen [verwenden Sie die andere Überladung von `invokeAPI`][5].  Um eine `GET`-Anforderung anstelle einer `POST`-Anforderung zu erstellen, setzen Sie den Parameter `HTTPMethod` auf `"GET"` und den Parameter `body` auf `nil` (da GET-Anforderungen keinen Nachrichtentext enthalten). Wenn Ihre benutzerdefinierte API andere HTTP-Verben unterstützt, ändern Sie `HTTPMethod` entsprechend.

**Objective-C:**

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift:**

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="<a-name="templates"></a>how-to:-register-push-templates-to-send-cross-platform-notifications"></a><a name="templates"></a>Gewusst wie: Registrieren von Pushvorlagen zum Senden plattformübergreifender Benachrichtigungen

Um Vorlagen zu registrieren, übergeben Sie die Vorlagen mit Ihrer **client.push registerDeviceToken** -Methode in Ihrer Client-App.

**Objective-C:**

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift:**

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Ihre Vorlagen sind vom Typ „NSDictionary“ und können mehrere Vorlagen im folgenden Format enthalten:

**Objective-C:**

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift:**

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Aus Sicherheitsgründen werden alle Tags aus der Anforderung entfernt.  Informationen zum Hinzufügen von Tags zu Installationen bzw. Vorlagen innerhalb von Installationen finden Sie unter [Verwenden des .NET-Back-End-Server SDK für Azure Mobile Apps][4].  Zum Senden von Benachrichtigungen, die diese registrierten Vorlagen verwenden, arbeiten Sie mit [Notification Hubs-APIs][3].

##<a name="<a-name="errors"></a>how-to:-handle-errors"></a><a name="errors"></a>Gewusst wie: Fehlerbehandlung

Beim Aufrufen eines mobilen Azure App Service-Back-Ends enthält der completion-Block einen `NSError` -Parameter. Wenn ein Fehler auftritt, ist der Wert dieses Parameters “non-nil“. Sie sollten diesen Parameter in Ihrem Code prüfen und Fehler entsprechend behandeln, wie in den vorigen Codeausschnitten veranschaulicht.

Die Datei [`<WindowsAzureMobileServices/MSError.h>`][6] definiert die Konstanten `MSErrorResponseKey`, `MSErrorRequestKey` und `MSErrorServerItemKey`. So erhalten Sie weitere Daten zum Fehler:

**Objective-C:**

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift:**

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Darüber hinaus definiert die Datei Konstanten für jeden Fehlercode:

**Objective-C:**

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift:**

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="<a-name="adal"></a>how-to:-authenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>Gewusst wie: Authentifizieren von Benutzern mit der Active Directory-Authentifizierungsbibliothek

Nutzen Sie die Active Directory-Authentifizierungsbibliothek (Active Directory Authentication Library, ADAL), um Benutzer mithilfe von Azure Active Directory bei Ihrer Anwendung anzumelden. Die Clientflussauthentifizierung über das SDK eines Identitätsanbieters ist der `loginWithProvider:completion:` -Methode vorzuziehen.  Die Clientflussauthentifizierung bietet eine bessere Bedienbarkeit und die Möglichkeit, zusätzliche Anpassungen vorzunehmen.

1. Konfigurieren Sie Ihr mobiles App-Back-End für die AAD-Anmeldung, indem Sie die im Tutorial [So konfigurieren Sie Ihre App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung][7] beschriebenen Schritte ausführen. Schließen Sie auch den optionalen Schritt zur Registrierung einer nativen Clientanwendung ab. Für iOS sollte der Umleitungs-URI die Form `<app-scheme>://<bundle-id>` aufweisen. Weitere Informationen finden Sie unter [Schnellstart mit der ADAL für iOS][8].

2. Installieren Sie die ADAL mithilfe von Cocoapods. Bearbeiten Sie Ihr Podfile, um folgende Definition zu integrieren, und ersetzen Sie dabei **YOUR-PROJECT** durch den Namen Ihres Xcode-Projekts:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   und der Pod:

        pod 'ADALiOS'

3. Führen Sie unter Verwendung des Terminals `pod install` im Verzeichnis mit Ihrem Projekt aus, und öffnen Sie den erstellten Xcode-Arbeitsbereich (nicht das Projekt).

4. Fügen Sie auf Grundlage der verwendeten Sprache den unten stehenden Code zu Ihrer Anwendung hinzu. Führen Sie für jede Sprache die folgenden Ersetzungen durch:

    * Ersetzen Sie **INSERT-AUTHORITY-HERE** durch den Namen des Mandanten, in dem Sie Ihre Anwendung bereitgestellt haben. Er hat das Format https://login.windows.net/contoso.onmicrosoft.com. Sie können diesen Wert von der Registerkarte „Domäne“ in Ihrer Azure Active Directory-Instanz im [klassischen Azure-Portal] kopieren.
    * Ersetzen Sie **INSERT-RESOURCE-ID-HERE** durch die Client-ID für Ihr mobiles App-Back-End. Sie können die Client-ID im Portal auf der Registerkarte **Erweitert** unter **Azure Active Directory-Einstellungen** abrufen.
    * Ersetzen Sie **INSERT-CLIENT-ID-HERE** durch die Client-ID, die Sie aus der nativen Clientanwendung kopiert haben.
    * Ersetzen Sie mithilfe des HTTPS-Schemas **INSERT-REDIRECT-URI-HERE** durch den Endpunkt _/.auth/login/done_ Ihrer Website. Dieser Wert sollte so ähnlich lauten wie _https://contoso.azurewebsites.net/.auth/login/done_.

**Objective-C:**

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift:**

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="<a-name="facebook-sdk"></a>how-to:-authenticate-users-with-the-facebook-sdk-for-ios"></a><a name="facebook-sdk"></a>Gewusst wie: Authentifizieren von Benutzern mit dem Facebook-SDK für iOS

Mithilfe des Facebook-SDKs für iOS können Sie Benutzer über Facebook bei Ihrer App anmelden.  Die Verwendung der Clientflussauthentifizierung ist der `loginWithProvider:completion:` -Methode vorzuziehen.  Die Clientflussauthentifizierung bietet eine bessere Bedienbarkeit und die Möglichkeit, zusätzliche Anpassungen vorzunehmen.

1. Konfigurieren Sie Ihr mobiles App-Back-End für die Facebook-Anmeldung, indem Sie die im Tutorial [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Facebook-Anmeldung][9] beschriebenen Schritte ausführen.

2. Installieren Sie das Facebook-SDK für iOS gemäß den Anweisungen unter [Facebook-SDK für iOS – Erste Schritte][10]. Anstatt eine App zu erstellen, können Sie die iOS-Plattform zu Ihrer vorhandenen Registrierung hinzufügen. 

3. Die Facebook-Dokumentation enthält im App-Delegat etwas Objective-C-Code. Wenn Sie **Swift** verwenden, können Sie die folgenden Übersetzungen für „AppDelegate.swift“ verwenden:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Fügen Sie Ihrem Projekt nicht nur `FBSDKCoreKit.framework` hinzu, sondern auch `FBSDKLoginKit.framework`, und zwar auf die gleiche Weise. 

4. Fügen Sie auf Grundlage der verwendeten Sprache den unten stehenden Code zu Ihrer Anwendung hinzu. 

**Objective-C:**

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift:**

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="<a-name="twitter-fabric"></a>how-to:-authenticate-users-with-twitter-fabric-for-ios"></a><a name="twitter-fabric"></a>Gewusst wie: Authentifizieren von Benutzern mit Twitter Fabric für iOS

Mithilfe von Fabric für iOS können Sie Benutzer über Twitter bei Ihrer App anmelden. Aufgrund der besseren Bedienbarkeit und der Möglichkeit, zusätzliche Anpassungen vorzunehmen, ist die Clientflussauthentifizierung der `loginWithProvider:completion:` -Methode vorzuziehen.

1. Konfigurieren Sie Ihr mobiles App-Back-End für die Twitter-Anmeldung, indem Sie die im Tutorial [Konfigurieren Ihrer App Service-Anwendung zur Nutzung der Twitter-Anmeldung](app-service-mobile-how-to-configure-twitter-authentication.md) beschriebenen Schritte ausführen.

2. Fügen Sie Fabric Ihrem Projekt hinzu, indem Sie die Anweisungen unter [Fabric für iOS – Erste Schritte] ausführen und das TwitterKit einrichten.

    > [AZURE.NOTE] Standardmäßig erstellt Fabric eine neue Twitter-Anwendung für Sie. Sie können die Erstellung einer Anwendung verhindern, indem Sie den zuvor erstellten Consumerschlüssel und das zuvor erstellte Consumergeheimnis mithilfe der folgenden Codeausschnitte registrieren.  Alternativ können Sie auch die Werte für Consumer Key und Consumer Secret, die für App Service bereitgestellt werden, durch die im [Fabric Dashboard] angezeigten Werte ersetzen. Wenn Sie sich für diese Möglichkeit entscheiden, müssen Sie die Rückruf-URL auf einen Platzhalterwert festlegen, z.B. auf `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Wenn Sie die zuvor erstellten Schlüssel und Geheimnisse verwenden, müssen Sie Ihrem App-Delegat Folgendes hinzufügen:
    
    **Objective-C:**

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift:**
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Fügen Sie auf Grundlage der verwendeten Sprache den unten stehenden Code zu Ihrer Anwendung hinzu. 

**Objective-C:**

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift:**

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="<a-name="google-sdk"></a>how-to:-authenticate-users-with-the-google-sign-in-sdk-for-ios"></a><a name="google-sdk"></a>Gewusst wie: Authentifizieren von Benutzern mit dem Google-Anmelde-SDK für iOS

Mithilfe des Google-Anmelde-SDKs für iOS können Sie Benutzer über ein Google-Konto bei Ihrer Anwendung anmelden.  Google kündigte vor Kurzem Änderungen an den OAuth-Sicherheitsrichtlinien an.  Aufgrund dieser Richtlinienänderungen ist in Zukunft die Verwendung des Google-SDK erforderlich.

1. Konfigurieren Sie Ihr mobiles App-Back-End für die Google-Anmeldung, indem Sie die im Tutorial [Konfigurieren Ihrer App Service-Anwendung zur Nutzung der Google-Anmeldung](app-service-mobile-how-to-configure-google-authentication.md) beschriebenen Schritte ausführen.

2. Installieren Sie das Google-SDK für iOS, indem Sie die Schritte in der Dokumentation [Google Sign-In for iOS - Start integrating](https://developers.google.com/identity/sign-in/ios/start-integrating) (Google-Anmeldung für iOS: Beginnen mit der Integration) ausführen. Sie können den Abschnitt zur Authentifizierung bei einem Back-End-Server überspringen.

3. Fügen Sie je nach verwendeter Sprache der `signIn:didSignInForUser:withError:`-Methode des Delegaten Folgendes hinzu.

**Objective-C:**

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift:**

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Fügen Sie zudem Folgendes zu `application:didFinishLaunchingWithOptions:` im App-Delegaten hinzu. Ersetzen Sie dabei „SERVER_CLIENT_ID“ durch die gleiche ID, die Sie zum Konfigurieren von App Service in Schritt 1 verwendet haben.

**Objective-C:**

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift:**
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Fügen Sie Ihrer Anwendung je nach verwendeter Sprache folgenden Code in einem UIViewController-Element hinzu, das das `GIDSignInUIDelegate` -Protokoll implementiert.  Sie werden ab- und wieder angemeldet, und obwohl Sie Ihre Anmeldeinformationen nicht erneut eingeben müssen, wird ein Zustimmungsdialogfeld angezeigt.  Rufen Sie diese Methode nur auf, wenn das Sitzungstoken abgelaufen ist.
 
 **Objective-C:**

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift:**
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[Was sind Mobile Services?]: #what-is
[Konzepte]: #concepts
[Einrichtung und Voraussetzungen]: #Setup
[Gewusst wie: Erstellen des Mobile Services-Clients]: #create-client
[Gewusst wie: Erstellen von Tabellenverweisen]: #table-reference
[Gewusst wie: Abfragen von Daten aus einem mobilen Dienst]: #querying
[Zurückgegebene Daten filtern]: #filtering
[Zurückgegebene Daten sortieren]: #sorting
[Daten seitenweise zurückgeben]: #paging
[Bestimmte Spalten auswählen]: #selecting
[Gewusst wie: Datenbindung in der Benutzeroberfläche]: #binding
[Einfügen von Daten in einen mobilen Dienst]: #inserting
[Ändern von Daten in einem mobilen Dienst]: #modifying
[Authentifizieren von Benutzern]: #authentication
[Zwischenspeichern von Authentifizierungstoken]: #caching-tokens
[Gewusst wie: Hochladen von Bildern und großen Dateien]: #blobs
[Fehlerbehandlung]: #errors
[Gewusst wie: Design von Komponententests]: #unit-testing
[Gewusst wie: Anpassen des Clients]: #customizing
[Anpassen der Anforderungsheader]: #custom-headers
[Anpassen der Datentyp-Serialisierung]: #custom-serialization
[Next Steps]: #next-steps
[Gewusst wie: Verwenden von MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Schnellstart von Azure Mobile Apps]: app-service-mobile-ios-get-started.md

[Hinzufügen von Mobile Services zu einer vorhandenen App]: /develop/mobile/tutorials/get-started-data
[Erste Schritte mit Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Überprüfen und Ändern von Daten in Mobile Services mithilfe von Serverskripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services-SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentifizierung]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Umgang mit abgelaufenen Token]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Berechtigungen]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Dienstseitige Autorisierung]: mobile-services-javascript-backend-service-side-authorization.md
[Verwenden von Skripts zum Autorisieren von Benutzern]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dynamische Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[Gewusst wie: Zugreifen auf benutzerdefinierte Parameter]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Erstellen einer Tabelle]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary-Objekt]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII-Steuerzeichen C0 und C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI zum Verwalten von Mobile Services-Tabellen]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Konflikthandler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Fabric Dashboard]: https://www.fabric.io/home
[Fabric for iOS – Getting Started]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started



<!--HONumber=Oct16_HO2-->


