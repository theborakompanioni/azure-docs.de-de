## <a name="prepare-your-intel-nuc"></a>Vorbereiten Ihres Intel NUC

Führen Sie folgende Schritte aus, um die Hardwareeinrichtung abzuschließen:

- Verbinden Sie Ihren Intel NUC mit dem im Kit enthaltenen Netzteil.
- Verbinden Sie Ihren Intel NUC per Ethernet-Kabel mit Ihrem Netzwerk.

Sie haben die Hardwareeinrichtung Ihres Intel NUC-Gatewaygeräts jetzt abgeschlossen.

### <a name="sign-in-and-access-the-terminal"></a>Anmelden und Zugreifen auf das Terminal

Sie haben zwei Möglichkeiten, um auf Ihrem Intel NUC auf eine Terminalumgebung zuzugreifen:

- Wenn Sie an Ihren Intel NUC eine Tastatur und einen Monitor angeschlossen haben, können Sie direkt auf die Shell zugreifen. Die Standardanmeldeinformationen lauten **root** für den Benutzernamen und **root** für das Kennwort.

- Greifen Sie auf die Shell auf Ihrem Intel NUC von Ihrem Desktopcomputer aus per SSH zu.

#### <a name="sign-in-with-ssh"></a>Anmelden mit SSH

Zum Anmelden per SSH benötigen Sie die IP-Adresse Ihres Intel NUC. Wenn Sie eine Tastatur und einen Monitor an Ihren Intel NUC angeschlossen haben, können Sie den Befehl `ifconfig` verwenden, um die IP-Adresse zu ermitteln. Alternativ dazu können Sie eine Verbindung mit Ihrem Router herstellen, um die Adressen der Geräte des Netzwerks aufzulisten.

Melden Sie sich mit dem Benutzernamen **root** und dem Kennwort **root** an.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>Optional: Freigeben eines Ordners auf Ihrem Intel NUC

Bei Bedarf können Sie auch einen Ordner auf Ihrem Intel NUC für Ihre Desktopumgebung freigeben. Wenn Sie einen Ordner freigeben, können Sie Dateien auf Ihrem Intel NUC mithilfe Ihres bevorzugten Desktop-Texteditors (z.B. [Visual Studio Code](https://code.visualstudio.com/) oder [Sublime Text](http://www.sublimetext.com/)) bearbeiten, anstatt `nano` oder `vi` zu verwenden.

Wenn Sie einen Ordner für Windows freigeben möchten, konfigurieren Sie einen Samba-Server auf dem Intel NUC. Alternativ dazu können Sie den SFTP-Server auf dem Intel NUC mit einem SFTP-Client auf Ihrem Desktopcomputer verwenden.
