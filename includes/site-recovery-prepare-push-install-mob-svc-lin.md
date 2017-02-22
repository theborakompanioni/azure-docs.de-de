### <a name="prepare-for-push-install-on-linux-servers"></a>Vorbereiten der Pushinstallation auf Linux-Servern

1. Stellen Sie sicher, dass zwischen dem Linux-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.
2. Erstellen Sie ein Konto, mit dem der Prozessserver auf den Computer zugreifen kann. Das Konto sollte ein **Root**-Benutzer auf dem Linux-Quellserver sein. (Das Konto wird nur für die Pushinstallation und Updates verwendet.)
3. Vergewissern Sie sich, dass die Datei `/etc/hosts` auf dem Linux-Quellserver Einträge enthält, die den Namen des lokalen Hosts den IP-Adressen zuordnen, die allen Netzwerkkarten zugewiesen sind.
4. Installieren Sie die neuesten „openssh“-, „openssh-server“- und „openssl“-Pakete auf dem Computer, den Sie replizieren möchten.
5. Stellen Sie sicher, dass SSH auf Port 22 aktiviert ist und ausgeführt wird.
6. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der Datei „sshd_config“ wie folgt:
  - Melden Sie sich als **Root**-Benutzer an.
  - Suchen Sie in der Datei „/etc/ssh/sshd_config“ die Zeile, die mit **PasswordAuthentication** beginnt.
  - Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert von **no** in **yes**.
   6.4 Suchen Sie nach der Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

7. Fügen Sie das Konto hinzu, das Sie in CSPSConfigtool erstellt haben.

    - Melden Sie sich am Konfigurationsserver an.
    - Öffnen Sie die Datei **cspsconfigtool.exe**. (Sie ist als Verknüpfung auf dem Desktop und unter dem Ordner „%ProgramData%\home\svsystems\bin“ verfügbar.)
    - Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    - Fügen Sie das von Ihnen erstellte Konto hinzu. Nach dem Hinzufügen des Kontos müssen Sie die Anmeldeinformationen angeben, wenn Sie die Replikation für einen Computer aktivieren.


<!--HONumber=Jan17_HO3-->


