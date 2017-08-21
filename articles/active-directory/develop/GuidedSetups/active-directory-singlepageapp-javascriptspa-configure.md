
### <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im Microsoft-Portal für die Anwendungsregistrierung registrieren:

1.  Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option *Geführtes Setup* aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3. Stellen Sie sicher, dass die Option *Geführtes Setup* deaktiviert ist.
4.  Klicken Sie auf `Add Platform`, und wählen Sie dann `Web` aus.
5. Fügen Sie eine Umleitungs-URL für Ihre Anwendung hinzu. Eine Umleitungs-URL ist die URL für Ihre Seite `index.html` basierend auf Ihrem Webserver.
6. Klicken Sie unten auf der Seite auf *Speichern*.

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url-using-ssl"></a>Visual Studio-Anweisungen zum Abrufen der Umleitungs-URL über SSL
> Wenn Sie Visual Studio verwenden, konfigurieren Sie Ihr Projekt zur Verwendung von SSL. Folgen Sie anschließend den nachstehenden Anweisungen, um die SSL-URL der Registrierungsinformationen Ihrer Anwendung zu konfigurieren:
> 1.    Wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie F4).
> 2.    Ändern Sie `SSL Enabled` in `True`.
> 3.    Kopieren Sie den Wert von `SSL URL` in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 4.    Wählen Sie das Menü `Project` und dann `{Project} Properties...` aus (hierbei steht {Project} für den Namen Ihres Projekts).
> 5.    Öffnen Sie die Registerkarte `Web`.
> 6.    Fügen Sie den Wert von `SSL URL` in das Feld `Project Url` ein.
> 7.    Wechseln Sie zurück zum Portal für die Anwendungsregistrierung, und fügen Sie den Wert in `Redirect URL` als Umleitungs-URL ein. Klicken Sie dann auf *Speichern*. 


#### <a name="configure-your-javascript-spa-application"></a>Konfigurieren Ihrer JavaScript-Einzelseitenanwendung

1.  Erstellen Sie eine Datei namens `msalconfig.js` mit den Informationen für die Anwendungsregistrierung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `JavaScript File`. Vergeben Sie den Namen `msalconfig.js`.
2.  Fügen Sie Ihrer Datei `msalconfig.js` den folgenden Code hinzu:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Ersetzen Sie <code>Enter_the_Application_Id_here</code> durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
</ol>