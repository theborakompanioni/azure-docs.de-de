## <a name="build-iot-edge"></a>Erstellen von IoT Edge

Dieses Tutorial verwendet benutzerdefinierte IoT Edge-Module für die Kommunikation mit der vorkonfigurierten Lösung für die Remoteüberwachung. Aus diesem Grund müssen Sie IoT Edge-Module aus benutzerdefiniertem Quellcode erstellen. In den folgenden Abschnitten wird beschrieben, wie IoT Edge installiert und das benutzerdefinierte IoT Edge-Modul erstellt wird.

### <a name="install-iot-edge"></a>Installieren von IoT Edge

Die folgenden Schritte beschreiben, wie die vorkompilierte IoT Edge-Software auf Intel NUC installiert wird:

1. Konfigurieren Sie die erforderlichen Smart Package-Repositorys durch Ausführen der folgenden Befehle auf Intel NUC:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Geben Sie bei der Eingabeaufforderung **Include this channel?** (Diesen Kanal einschließen?) `y` ein.

1. Aktualisieren Sie den Smart Package Manager durch Ausführen des folgenden Befehls:

    ```bash
    smart update
    ```

1. Installieren Sie das Azure IoT Edge-Paket durch Ausführen des folgenden Befehls:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. Überprüfen Sie die Installation durch Ausführen des Hello World-Beispiels. Mit diesem Beispiel wird alle fünf Sekunden eine Hello World-Nachricht in die Datei „log.txt“ geschrieben. Die folgenden Befehle führen das Hello World-Beispiel aus:

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Ignorieren Sie alle Meldungen zu **ungültigen Argumenten**, wenn Sie das Beispiel beenden.

    Sie können den Inhalt der Protokolldatei mit dem folgenden Befehl anzeigen:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Problembehandlung

Wenn der Fehler ausgegeben wird, dass kein Paket „util-linux-dev“ bereitstellt, starten Sie Intel NUC neu.
