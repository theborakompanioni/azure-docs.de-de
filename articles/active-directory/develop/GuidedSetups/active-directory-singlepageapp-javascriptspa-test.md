
## <a name="test-your-code"></a>Testen Ihres Codes

Wenn Sie Visual Studio verwenden, drücken Sie `F5`, um Ihr Projekt auszuführen. Der Browser wird geöffnet und leitet Sie zu *http://localhost: {Port}*, wo Sie die Schaltfläche *Microsoft Graph-API aufrufen* sehen.

Wenn Sie nicht Visual Studio verwenden, stellen Sie sicher, dass Ihr Webserver gestartet wurde und dass der Ordner mit Ihrer JavaScript-Webanwendung in Ihrem Webserver konfiguriert ist. Öffnen Sie den Browser, und geben Sie *http://localhost:{Port}/Pfad* ein. Hierbei entspricht *Port* dem Port, auf dem Ihr Webserver lauscht, und *Pfad* gibt den Pfad zu Ihrer Datei „index.html“ an.

Klicken Sie auf die Schaltfläche *Microsoft Graph-API aufrufen*. Wenn dies der erste Aufruf ist, werden Sie in einem Popupfenster zur Benutzeranmeldung aufgefordert.
 
![Beispielscreenshot](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Zustimmung
Bei der ersten Anmeldung bei Ihrer Anwendung wird ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt, auf dem Sie explizit Ihre Zustimmung geben müssen:

 ![Genehmigungsbildschirm](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)

Da Sie die Microsoft Graph-API abfragen, wird möglicherweise ein anderer Genehmigungsbildschirm angezeigt. Dies geschieht aufgrund der *dynamischen Genehmigung* – hierbei ist eine Genehmigung für jeden Bereich erforderlich, der von der Anwendung angefordert wird. Für die in diesem Leitfaden generierte Beispielanwendung wird der Bereich *user.read* bereitgestellt, deshalb müssen Sie der Anwendung Ihre Genehmigung zum Lesen des Benutzerprofils erteilen.

> [!IMPORTANT]
> Aktuell müssen Sie aufgrund eines bekannten Fehlers mit *msal* für JavaScript den Popupblocker in Browsern wie Chrome und Firefox deaktivieren, damit der Bildschirm für die *dynamische Genehmigung* ordnungsgemäß arbeitet. Der Popupblocker muss für den ersten Benutzeraufruf der Microsoft Graph-API unter Verwendung von `acquireTokenPopup` deaktiviert werden.

### <a name="expected-results"></a>Erwartete Ergebnisse
Es sollten Benutzerprofilinformationen angezeigt werden, die als Antwort auf den Microsoft Graph-API-Aufruf zurückgegeben werden.
 
 ![Ergebnisse](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Es werden außerdem in den Feldern *Zugriffstoken* und *ID-Tokenansprüche* grundlegende Informationen zum abgerufenen Token angezeigt.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich `user.read`, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die in unserem Registrierungsportal registriert wird. Einige andere APIs für Microsoft Graph sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern möglicherweise zusätzliche Bereiche. Beispielsweise wird für Microsoft Graph der Bereich `Calendars.Read` benötigt, um die Benutzerkalender aufzulisten. Um auf den Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung `Calendars.Read` hinzufügen und dann dem Aufruf von `acquireTokenSilent` den Bereich `Calendars.Read` hinzufügen. Wenn Sie die Anzahl von Bereichen erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich erfordert (nicht empfohlen), können Sie `clientId` als Bereich in den Aufrufen `acquireTokenSilent` und/oder `acquireTokenPopup` verwenden.

<!--end-collapse-->
