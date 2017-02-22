1. Führen Sie die Installationsdatei für das einheitliche Setup aus.
2. Wählen Sie unter **Vorbereitung** die Option **Install the configuration server and process server** (Konfigurationsserver und Prozessserver installieren) aus.
    ![Vorbereitung](./media/site-recovery-add-configuration-server/combined-wiz1.png)
3. Klicken Sie unter **Third-Party Software License** (Drittanbietersoftwarelizenz) auf **I Accept** (Ich stimme zu), um MySQL herunterzuladen und zu installieren.

    ![Drittanbietersoftware](./media/site-recovery-add-configuration-server/combined-wiz105.PNG)
4. Wählen Sie unter **Registration** (Registrierung) den Registrierungsschlüssel aus, den Sie aus dem Tresor heruntergeladen haben.

    ![Registrierung](./media/site-recovery-add-configuration-server/combined-wiz3.png)
5. Geben Sie unter **Internet Settings** (Interneteinstellungen) an, wie der auf dem Konfigurationsserver ausgeführte Anbieter eine Internetverbindung mit Azure Site Recovery herstellen soll.

   * Wenn die Verbindung über den derzeit auf dem Computer eingerichteten Proxy hergestellt werden soll, wählen Sie **Connect with existing proxy settings** (Mit vorhandenen Proxyeinstellungen verbinden) aus.
   * Wenn der Anbieter eine direkte Verbindung herstellen soll, wählen Sie **Connect directly without a proxy** (Ohne Proxy direkt verbinden) aus.
   * Falls für den vorhandenen Proxy eine Authentifizierung erforderlich ist oder Sie für die Anbieterverbindung einen benutzerdefinierten Proxy verwenden möchten, wählen Sie **Mit benutzerdefinierten Proxyeinstellungen verbinden**aus.

     * Bei Verwendung eines benutzerdefinierten Proxys müssen Sie die Adresse, den Port und die Anmeldeinformationen eingeben.
     * Bei Verwendung eines Proxys sollten Sie die unter [Voraussetzungen](#configuration-server-prerequisites) beschriebenen URLs bereits zugelassen haben.

     ![Firewall](./media/site-recovery-add-configuration-server/combined-wiz4.png)
6. Bei der **Voraussetzungsüberprüfung** führt das Setup eine Überprüfung durch, um sicherzustellen, dass die Installation ausgeführt werden kann. Falls beim **Überprüfen der Synchronisierung der globalen Zeit** eine Warnung angezeigt wird, stellen Sie sicher, dass die Zeit der Systemuhr (Einstellungen für **Datum und Uhrzeit**) mit der Zeitzone übereinstimmt.

    ![Voraussetzungen](./media/site-recovery-add-configuration-server/combined-wiz5.png)
7. Erstellen Sie unter **MySQL Configuration** (MySQL-Konfiguration) Anmeldeinformationen für die Anmeldung bei der MySQL-Serverinstanz, die installiert wird.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz6.png)
8. Wählen Sie unter **Umgebungsdetails** aus, ob Sie VMware-VMs replizieren werden. Wenn dies der Fall ist, überprüft das Setup, ob PowerCLI 6.0 installiert ist.

    ![MySQL](./media/site-recovery-add-configuration-server/combined-wiz7.png)

9. Wählen Sie unter **Install Location** (Installationsspeicherort) aus, wo die Binärdateien installiert werden sollen und wo der Cache gespeichert werden soll. Auf dem ausgewählten Laufwerk müssen mindestens 5 GB an Speicherplatz verfügbar sind, wir raten Ihnen jedoch zu einem Cachelaufwerk mit mindestens 600 GB freiem Speicherplatz.

    ![Installationsspeicherort](./media/site-recovery-add-configuration-server/combined-wiz8.png)
10. Geben Sie unter **Network Selection** (Netzwerkauswahl) den Listener (Netzwerkadapter und SSL-Port) an, über den der Konfigurationsserver Replikationsdaten sendet und empfängt. Port 9443 ist der Standardport zum Senden und Empfangen von Replikationsdatenverkehr. Sie können diese Portnummer jedoch ändern, um sie an die Anforderungen Ihrer Umgebung anzupassen. Zusätzlich zum Port 9443 öffnen wir auch den Port 443, der von einem Webserver zum Orchestrieren von Replikationsvorgängen verwendet wird. Verwenden Sie den Port 443 nicht, um Replikationsdatenverkehr zu senden oder zu empfangen.

    ![Netzwerkauswahl](./media/site-recovery-add-configuration-server/combined-wiz9.png)


11. Lesen Sie die unter **Summary** (Zusammenfassung) angezeigten Informationen und klicken Sie auf **Install** (Installieren). Nach Abschluss der Installation wird eine Passphrase generiert. Diese benötigen Sie bei der Aktivierung der Replikation. Kopieren Sie sie deshalb, und bewahren Sie sie an einem sicheren Ort auf.

    ![Zusammenfassung](./media/site-recovery-add-configuration-server/combined-wiz10.png)

Nach Abschluss der Registrierung wird der Server auf dem Blatt **Einstellungen** > **Server** im Tresor angezeigt.


<!--HONumber=Feb17_HO2-->


