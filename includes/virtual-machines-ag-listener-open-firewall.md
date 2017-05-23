In diesem Schritt erstellen Sie eine Firewallregel, um den Testport für den Endpunkt mit Lastenausgleich (59999 wie bereits angegeben) zu öffnen, und eine weitere Regel, um den Port des Verfügbarkeitsgruppenlisteners zu öffnen. Da Sie den Endpunkt mit Lastenausgleich auf den virtuellen Computern erstellt haben, die Verfügbarkeitsgruppenreplikate enthalten, müssen Sie den Testport und den Listenerport auf den entsprechenden virtuellen Computern öffnen.

1. Starten Sie auf virtuellen Computern, die Replikate hosten, **Windows-Firewall mit erweiterter Sicherheit**.

2. Klicken Sie mit der rechten Maustaste auf **Eingehende Regeln**, und klicken Sie dann auf **Neue Regel**.

3. Wählen Sie auf der Seite **Regeltyp** die Option **Port** aus, und klicken Sie dann auf **Weiter**.

4. Wählen Sie auf der Seite **Protokoll und Ports** die Option **TCP** aus, geben Sie **59999** in das Feld **Bestimmte lokale Ports** ein, und klicken Sie dann auf **Weiter**.

5. Lassen Sie auf der Seite **Aktion** die Option **Verbindung zulassen** aktiviert, und klicken Sie dann auf **Weiter**.

6. Akzeptieren Sie auf der Seite **Profil** die Standardeinstellungen, und klicken Sie dann auf **Weiter**.

7. Geben Sie auf der Seite **Name** in das Textfeld **Name** einen Regelnamen ein, z.B. **AlwaysOn-Listenertestport**, und klicken Sie dann auf **Fertig stellen**.

8. Wiederholen Sie die vorherigen Schritte für den Port des Verfügbarkeitsgruppenlisteners (wie weiter oben im $EndpointPort-Parameter des Skripts angegeben), und geben Sie dann einen geeigneten Regelnamen an, z.B. **AlwaysOn-Listenerport**.

