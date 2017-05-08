## <a name="prepare-your-raspberry-pi"></a>Vorbereiten des Raspberry Pi

### <a name="install-raspbian"></a>Installieren von Raspbian

Wenn Sie den Raspberry Pi zum ersten Mal verwenden, müssen Sie das Raspbian-Betriebssystem mithilfe von NOOBS auf der im Kit enthaltenen SD-Karte installieren. Eine entsprechende Installationsanleitung finden Sie im [Softwarehandbuch für den Raspberry Pi][lnk-install-raspbian]. In diesem Tutorial wird davon ausgegangen, dass Sie das Raspbian-Betriebssystem bereits auf Ihrem Raspberry Pi installiert haben.

> [!NOTE]
> Auf der SD-Karte des [Microsoft Azure IoT Starter Kits für Raspberry Pi 3][lnk-starter-kits] ist NOOBS bereits installiert. Sie können den Raspberry Pi über diese Karte starten und das Raspbian-Betriebssystem installieren.

Führen Sie folgende Schritte aus, um die Hardwareeinrichtung abzuschließen:

- Verbinden Sie Ihren Raspberry Pi mit dem im Kit enthaltenen Netzteil.
- Verbinden Sie Ihren Raspberry Pi über das im Kit enthaltene Ethernetkabel mit Ihrem Netzwerk. Alternativ können Sie eine [WLAN-Verbindung][lnk-pi-wireless] für Ihren Raspberry Pi einrichten.

Damit ist die Hardwareeinrichtung Ihres Raspberry Pi abgeschlossen.

### <a name="sign-in-and-access-the-terminal"></a>Anmelden und Zugreifen auf das Terminal

Sie können auf Ihrem Raspberry Pi auf zwei Arten auf eine Terminalumgebung zuzugreifen:

- Wenn Sie eine Tastatur und einen Monitor an Ihren Raspberry Pi angeschlossen haben, können Sie über die grafische Raspbian-Benutzeroberfläche auf ein Terminalfenster zugreifen.

- Sie können aber auch auf Ihrem Desktopcomputer per SSH auf die Befehlszeile Ihres Raspberry Pi zugreifen.

#### <a name="use-a-terminal-window-in-the-gui"></a>Verwenden eines Terminalfensters auf der grafischen Benutzeroberfläche

Die Anmeldeinformationen für Raspbian lauten standardmäßig **pi** (Benutzername) und **raspberry** (Kennwort). Auf der Taskleiste der grafischen Benutzeroberfläche können Sie über das Monitorsymbol das Hilfsprogramm **Terminal** starten.

#### <a name="sign-in-with-ssh"></a>Anmelden mit SSH

Sie können über SSH auf die Befehlszeile Ihres Raspberry Pi zugreifen. Im Artikel [SSH (Secure Shell)][lnk-pi-ssh] erfahren Sie, wie Sie SSH auf Ihrem Raspberry Pi konfigurieren und unter [Windows][lnk-ssh-windows] oder unter [Linux und Mac OS][lnk-ssh-linux] eine Verbindung herstellen.

Melden Sie sich mit dem Benutzernamen **pi** und dem Kennwort **raspberry** an.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Optional: Freigeben eines Ordners auf Ihrem Raspberry Pi

Bei Bedarf können Sie auch einen Ordner auf Ihrem Raspberry Pi für Ihre Desktopumgebung freigeben. Wenn Sie einen Ordner freigeben, können Sie Dateien auf Ihrem Raspberry PI mithilfe Ihres bevorzugten Desktop-Texteditors (etwa [Visual Studio Code](https://code.visualstudio.com/) oder [Sublime Text](http://www.sublimetext.com/)) bearbeiten, anstatt `nano` oder `vi` zu verwenden.

Wenn Sie einen Ordner für Windows freigeben möchten, konfigurieren Sie einen Samba-Server auf dem Raspberry Pi. Alternativ können Sie den integrierten [SFTP](https://www.raspberrypi.org/documentation/remote-access/)-Server mit einem SFTP-Client auf Ihrem Desktop verwenden.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/