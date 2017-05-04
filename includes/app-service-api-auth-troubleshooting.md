Der Großteil der Authentifizierungsfehler ergibt sich aus falschen oder inkonsistenten Konfigurationseinstellungen. Im folgenden finden Sie einige Vorschläge zur Überprüfung:

* Stellen Sie sicher, dass Sie überall auf **Speichern** geklickt haben. Das geht oft ganz schnell, und das Ergebnis ist, dass Sie zwar die korrekten Werte auf einer Portalseite sehen, diese jedoch tatsächlich nicht in der Azure-Umgebung oder Azure-AD-Anwendung gespeichert wurden.
* Stellen Sie für die Einstellungen auf dem Blatt **Anwendungseinstellungen** im Azure-Portal sicher, dass die richtige API-App oder Web-App ausgewählt war, als die Einstellungen eingegeben wurden.  Vergewissern Sie sich außerdem, dass die Einstellungen als **App-Einstellungen** und nicht als **Verbindungszeichenfolgen** eingegeben wurden, denn das Format der beiden Abschnitte ist recht ähnlich.
* Laden Sie für die Authentifizierung bei einem JavaScript-Front-End das Manifest erneut herunter, und vergewissern Sie sich, dass `oauth2AllowImplicitFlow` erfolgreich in `true` geändert wurde.
* Stellen Sie sicher, dass Sie HTTPS verwendet haben, wo immer Sie URLs konfiguriert haben:
  
  * Im Projektcode
  * In CORS
  * In den App-Einstellungen der Azure-Umgebung für jede API-App und Web-App
  * In den Anwendungseinstellungen von Azure AD
    
    Wenn Sie eine URL einer API-App aus dem Portal kopieren, kann es vorkommen, dass diese mit `http://` beginnt und manuell in `https://` geändert werden muss.
* Stellen Sie sicher, dass alle Änderungen am Code erfolgreich bereitgestellt wurden. In einer Projektmappe mit mehreren Projekte ist es beispielsweise möglich, einen Projektcode zu ändern und dabei versehentlich einen der anderen zu wählen, wenn Sie die Änderung bereitstellen möchten.
* Achten Sie darauf, dass Sie in Ihrem Browser auf HTTPS-URLs zugreifen, nicht auf HTTP-URLs. Standardmäßig erstellt Visual Studio Veröffentlichungsprofile mit HTTP-URLs, und diese werden im Browser geöffnet, nachdem Sie ein Projekt bereitstellen.
* Stellen Sie bei einer Authentifizierung zu einem JavaScript-Front-End sicher, dass CORS korrekt auf der API-App, die der JavaScript-Code aufruft, konfiguriert ist. Falls Sie unsicher sind, ob das Problem mit CORS zusammenhängt, können Sie versuchen, „*“ als zulässige Ursprungs-URL zu verwenden. 
* Für ein JavaScript-Front-End, öffnen Sie die Registerkarte mit der Entwicklertools-Konsole, um weitere Fehlerinformationen zu erhalten und untersuchen Sie die HTTP-Anforderungen auf dem Netzwerk. Die Fehlermeldungen der Konsole können jedoch irreführend sein. Wenn Sie eine Meldung erhalten, die auf einen CORS-Fehler hinweist, könnte der Fehler bei der Authentifizierung liegen. Sie können dies überprüfen, indem Sie die App mit vorübergehend deaktivierter Authentifizierung ausführen.
* Stellen Sie bei einer .NET-API-App sicher, dass Sie in Fehlermeldungen so viele Informationen wie möglich erhalten, indem Sie [den customErrors-Modus auf „Off“ festlegen](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Starten Sie bei einer .NET-API-App eine [Remotedebuggingsitzung](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), und überprüfen Sie die Werte der Variablen, die an Code übergeben werden, der ADAL verwendet, um ein Bearertoken zu beziehen, oder an Code, der Ansprüche für die erwartete Dienstprinzipal-ID überprüft. Beachten Sie, dass mit Ihrem Code die Konfigurationswerte aus vielen unterschiedlichen Quellen erfasst werden können, sodass Sie hier ggf. auch Überraschungen erleben. Wenn Sie beim Konfigurieren von Azure App Service-Umgebungseinstellungen beispielsweise nicht `ida:ClientId` angeben, sondern `ida:ClientID`, kann Folgendes passieren: Der Code ruft den gesuchten Wert für `ida:ClientId` aus der Datei „Web.config“ ab und ignoriert die Azure App Service-Einstellung. 
* Wenn etwas nicht in einem normalen Fenster in Internet Explorer funktioniert, könnte eine existierende Anmeldung stören. Versuchen Sie es noch einmal im InPrivate-, Chrome- oder Firefox-Browser.

