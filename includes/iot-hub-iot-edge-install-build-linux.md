## <a name="install-the-prerequisites"></a>Installieren der erforderlichen Komponenten

Bei den Schritten in diesem Tutorial wird davon ausgegangen, dass Sie Ubuntu Linux ausführen.

Öffnen Sie eine Shell, und führen Sie die folgenden Befehle zum Installieren der erforderlichen Pakete aus:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

Führen Sie in der Shell den folgenden Befehl aus, um das Azure IoT Edge-GitHub-Repository auf Ihrem lokalen Computer zu klonen:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a>Erstellen des Beispiels

Sie können nun die IoT Edge-Laufzeit und Beispiele auf dem lokalen Computer erstellen:

1. Öffnen Sie eine Shell.

1. Navigieren Sie zum Stammordner in Ihrer lokalen Kopie des Repositorys **iot-edge**.

1. Führen Sie das Skript zum Erstellen wie folgt aus:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Dieses Skript verwendet das Hilfsprogramm **cmake**, um einen Ordner namens **build** im Stammordner Ihrer lokalen Kopie des Repositorys **iot-edge** zu erstellen und ein Makefile zu generieren. Das Skript erstellt dann die Lösung und überspringt die Komponententests und End-to-End-Tests. Fügen Sie den Parameter `--run-unittests` hinzu, wenn Sie die Komponententests erstellen und ausführen möchten. Fügen Sie `--run-e2e-tests` hinzu, wenn Sie die End-to-End-Tests erstellen und ausführen möchten.

> [!NOTE]
> Bei jeder Ausführung löscht das Skript **build.sh** den Ordner **build** im Stammordner Ihrer lokalen Kopie des Repositorys **iot-edge** und erstellt ihn neu.