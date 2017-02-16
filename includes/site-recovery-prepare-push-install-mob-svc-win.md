### <a name="prepare-for-push-install-on-windows-machines"></a>Vorbereiten für Pushinstallation auf Windows-Computern

1. Stellen Sie sicher, dass zwischen dem Windows-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.
2. Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto muss über Administratorrechte (lokal oder für die Domäne) verfügen, und es wird nur für die Pushinstallation und Agent-Updates verwendet.

   > [!NOTE]
   > Wenn Sie kein Domänenkonto verwenden, müssen Sie die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Sie können die Remote-Benutzerzugriffssteuerung auf einem Computer deaktivieren, indem Sie das neue DWORD **LocalAccountTokenFilterPolicy** unter dem Registrierungsschlüssel „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System“ hinzufügen und dessen Wert auf 1 festlegen. Dies ist der Befehl, den Sie zu diesem Zweck an einer Befehlseingabeaufforderung verwenden können: **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.
   >
   >
2. Wählen Sie in der Windows-Firewall des Computers, den Sie schützen möchten, **Eine App oder ein Feature durch die Windows-Firewall zulassen**aus. Aktivieren Sie **Datei- und Druckerfreigabe** und **Windows-Verwaltungsinstrumentation**. Für Computer, die einer Domäne angehören, können Sie die Firewalleinstellungen mit einem Gruppenrichtlinienobjekt konfigurieren.

   ![Firewalleinstellungen](./media/site-recovery-prepare-push-install-mob-svc-win/mobility1.png)

3. Fügen Sie das Konto hinzu, das Sie in CSPSConfigtool erstellt haben.
    - Melden Sie sich am Konfigurationsserver an.
    - Öffnen Sie die Datei **cspsconfigtool.exe**. (Sie ist als Verknüpfung auf dem Desktop und unter dem Ordner „%ProgramData%\home\svsystems\bin“ verfügbar.)
    - Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    - Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.


<!--HONumber=Jan17_HO3-->


