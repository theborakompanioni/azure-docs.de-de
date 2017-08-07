### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Vorbereiten einer Pushinstallation auf einem Linux-Server

1. Stellen Sie sicher, dass zwischen dem Linux-Computer und dem Prozessserver eine Netzwerkkonnektivität besteht.
2. Erstellen Sie ein Konto, das vom Prozessserver zum Zugreifen auf den Computer verwendet werden kann. Das Konto muss einem **Root**-Benutzer auf dem Linux-Quellserver entsprechen. (Verwenden Sie dieses Konto nur für die Pushinstallation und für Updates.)
3. Vergewissern Sie sich, dass die Datei „/etc/hosts“ auf dem Linux-Quellserver Einträge enthält, mit denen die Namen des lokalen Hosts den IP-Adressen zugeordnet werden, die allen Netzwerkkarten zugeordnet sind.
4. Installieren Sie die neuesten openssh-, openssh-server- und openssl-Pakete auf dem Computer, den Sie replizieren möchten.
5. Stellen Sie sicher, dass Secure Shell (SSH) auf Port 22 aktiviert ist und ausgeführt wird.
6. Aktivieren Sie das SFTP-Subsystem und die Kennwortauthentifizierung in der sshd_config-Datei:
  1.  Melden Sie sich als **Root**-Benutzer an.
  2.  Suchen Sie in der Datei „/etc/ssh/sshd_config“ die Zeile, die mit **PasswordAuthentication** beginnt.
  3.  Heben Sie die Auskommentierung der Zeile auf, und ändern Sie den Wert in **yes**.
  4.  Suchen Sie die Zeile, die mit **Subsystem** beginnt, und heben Sie die Auskommentierung der Zeile auf.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Starten Sie den Dienst **sshd** neu.

7. Fügen Sie das Konto hinzu, das Sie im CSPSConfigtool erstellt haben.
    1.  Melden Sie sich am Konfigurationsserver an.
    2.  Öffnen Sie die Datei **cspsconfigtool.exe**. (Sie ist als Verknüpfung auf dem Desktop und im Ordner „%ProgramData%\home\svsystems\bin“ verfügbar.)
    3.  Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.
    4.  Fügen Sie das von Ihnen erstellte Konto hinzu. 
    5.  Geben Sie die Anmeldeinformationen ein, die Sie verwenden, wenn Sie die Replikation für einen Computer aktivieren.
