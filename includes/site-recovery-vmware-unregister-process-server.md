Die Schritte zum Aufheben der Registrierung eines Prozessservers sind abhängig vom Status der Verbindung mit dem Konfigurationsserver.

### <a name="unregister-a-process-server-that-is-in-a-connected-state"></a>Aufheben der Registrierung eines verbundenen Prozessservers

1. Stellen Sie als Administrator eine Remoteverbindung mit dem Prozessserver her.
2. Starten Sie die **Systemsteuerung**, und öffnen Sie **Programme > Programm deinstallieren**.
3. Deinstallieren Sie das Programm **Microsoft Azure Site Recovery Configuration/Process Server** (Microsoft Azure Site Recovery-Konfiguration/Prozessserver).
4. Nach Abschluss von Schritt 3 können Sie **Microsoft Azure Site Recovery Configuration/Process Server Dependencies** (Microsoft Azure Site Recovery-Konfiguration/Prozessserverabhängigkeiten) deinstallieren.

### <a name="unregister-a-process-server-that-is-in-a-disconnected-state"></a>Aufheben der Registrierung eines nicht verbundenen Prozessservers

> [!WARNING]
> Gehen Sie wie folgt vor, falls sich der virtuelle Computer, auf dem der Prozessserver installiert wurde, nicht reaktivieren lässt:

1. Melden Sie sich als Administrator bei Ihrem Konfigurationsserver an.
2. Öffnen Sie eine Administratoreingabeaufforderung, und navigieren Sie zum Verzeichnis `%ProgramData%\ASR\home\svsystems\bin`.
3. Führen Sie nun den folgenden Befehl aus:

    ```
    perl Unregister-ASRComponent.pl -IPAddress <IP_of_Process_Server> -Component PS
    ```
4. Dadurch werden die Details des Prozessservers aus dem System entfernt.
