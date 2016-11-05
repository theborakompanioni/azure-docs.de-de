**Objective-C**:

1. Öffnen Sie auf dem Mac *QSTodoListViewController.m* in Xcode, und fügen Sie folgende Methode hinzu. Wenn Sie Google nicht als Identitätsanbieter nutzen, ändern Sie *google* zu *microsoftaccount*, *twitter*, *facebook* oder *windowsazureactivedirectory*. Wenn Sie Facebook verwenden, [müssen Sie die Facebook-Domänen der Positivliste in Ihrer App hinzufügen](https://developers.facebook.com/docs/ios/ios9#whitelist).
   
            - (void) loginAndGetData
            {
                MSClient *client = self.todoService.client;
                if (client.currentUser != nil) {
                    return;
                }
   
                [client loginWithProvider:@"google" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                }];
            }
2. In *QSTodoListViewController.m* ersetzen Sie `[self refresh]` in `viewDidLoad` durch Folgendes:
   
            [self loginAndGetData];
3. Klicken Sie *Ausführen*, um die App zu starten und melden Sie sich an. Nach der Anmeldung sollten Sie die Todo-Liste anzeigen und Änderungen vornehmen können.

**Swift**:

1. Öffnen Sie auf dem Mac *ToDoTableViewController.swift* in Xcode, und fügen Sie folgende Methode hinzu. Wenn Sie Google nicht als Identitätsanbieter nutzen, ändern Sie *google* zu *microsoftaccount*, *twitter*, *facebook* oder *windowsazureactivedirectory*. Wenn Sie Facebook verwenden, [müssen Sie die Facebook-Domänen der Positivliste in Ihrer App hinzufügen](https://developers.facebook.com/docs/ios/ios9#whitelist).
   
            func loginAndGetData() {
   
                guard let client = self.table?.client where client.currentUser == nil else {
                    return
                }
   
                client.loginWithProvider("google", controller: self, animated: true) { (user, error) in
                    self.refreshControl?.beginRefreshing()
                    self.onRefresh(self.refreshControl)
                }
            }
2. Entfernen Sie in *ToDoTableViewController.swift* die Zeilen `self.refreshControl?.beginRefreshing()` und `self.onRefresh(self.refreshControl)` am Ende von `viewDidLoad()`. Fügen Sie an deren Stelle einen Aufruf von `loginAndGetData()` ein:
   
            loginAndGetData()
3. Klicken Sie *Ausführen*, um die App zu starten und melden Sie sich an. Nach der Anmeldung sollten Sie die Todo-Liste anzeigen und Änderungen vornehmen können.

<!---HONumber=AcomDC_0218_2016-->