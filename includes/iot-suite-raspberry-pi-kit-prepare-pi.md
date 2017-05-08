## <a name="prepare-your-raspberry-pi"></a>Vorbereiten des Raspberry Pi

### <a name="install-raspbian"></a>Installieren von Raspbian

Wenn Sie den Raspberry Pi zum ersten Mal verwenden, müssen Sie das Raspbian-Betriebssystem mithilfe von NOOBS auf der im Kit enthaltenen SD-Karte installieren. Eine entsprechende Installationsanleitung finden Sie im [Softwarehandbuch für den Raspberry Pi][lnk-install-raspbian]. In diesem Tutorial wird davon ausgegangen, dass Sie das Raspbian-Betriebssystem bereits auf Ihrem Raspberry Pi installiert haben.

> [!NOTE]
> Auf der SD-Karte des [Microsoft Azure IoT Starter Kits für Raspberry Pi 3][lnk-starter-kits] ist NOOBS bereits installiert. Sie können den Raspberry Pi über diese Karte starten und das Raspbian-Betriebssystem installieren.

### <a name="set-up-the-hardware"></a>Einrichten der Hardware

In diesem Tutorial werden mit dem BME280-Sensor aus dem [Microsoft Azure IoT Starter Kit für Raspberry Pi 3][lnk-starter-kits] Telemetriedaten generiert. Mithilfe einer LED wird angegeben, wann der Raspberry Pi einen Methodenaufruf des Lösungsdashboards verarbeitet.

Komponenten auf dem Breadboard:

- Rote LED
- 220-Ohm-Widerstand (Rot, Rot, Braun)
- BME280-Sensor

Anschlussdiagramm für Ihre Hardware:

![Hardwareeinrichtung für Raspberry Pi][img-connection-diagram]

In der folgenden Tabelle sind die Verbindungen zwischen dem Raspberry Pi und den Komponenten auf dem Breadboard zusammengefasst:

| Raspberry Pi            | Breadboard             |Farbe         |
| ----------------------- | ---------------------- | ------------- |
| GND (Pin 14)            | LED -ve pin (18A)      | Violett          |
| GPCLK0 (Pin 7)          | Widerstand (25A)         | Orange          |
| SPI_CE0 (Pin 24)        | CS (39A)               | Blau          |
| SPI_SCLK (Pin 23)       | SCK (36A)              | Gelb        |
| SPI_MISO (Pin 21)       | SDO (37A)              | Weiß         |
| SPI_MOSI (Pin 19)       | SDI (38A)              | Grün         |
| GND (Pin 6)             | GND (35A)              | Schwarz         |
| 3,3 V (Pin 1)           | 3Vo (34A)              | Rot           |

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

### <a name="enable-spi"></a>Aktivieren der SPI

Um die Beispielanwendung ausführen zu können, muss auf dem Raspberry Pi zunächst der SPI-Bus (Serial Peripheral Interface) aktiviert werden. Der Raspberry Pi kommuniziert über den SPI-Bus mit dem BME280-Sensor. Verwenden Sie den folgenden Befehl, um die Konfigurationsdatei zu bearbeiten:

`sudo nano /boot/config.txt`

Suchen Sie die folgende Zeile:

```
#dtparam=spi=on
```

- Löschen das Zeichen `#` am Anfang, um die Auskommentierung der Zeile aufzuheben.
- Speichern Sie Ihre Änderungen (**STRG+O**, **EINGABETASTE**), und beenden Sie den Editor (**STRG+X**).
- Starten Sie den Raspberry Pi neu, um die SPI zu aktivieren. Da durch den Neustart die Verbindung mit dem Terminal getrennt wird, müssen Sie sich nach dem Neustart des Raspberry Pi erneut anmelden:

  `sudo reboot`


[img-connection-diagram]: media/iot-suite-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/