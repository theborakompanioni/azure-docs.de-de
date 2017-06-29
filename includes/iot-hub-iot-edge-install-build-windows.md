## <a name="install-the-prerequisites"></a>Installieren der erforderlichen Komponenten

1. Installieren Sie [Visual Studio 2015 oder 2017](https://www.visualstudio.com). Sie können die kostenlose Community Edition verwenden, wenn die Lizenzierungsanforderungen erfüllt sind. Achten Sie darauf, dass Visual C++ und NuGet-Paket-Manager enthalten sind.

1. Installieren Sie [git](http://www.git-scm.com), und stellen Sie sicher, dass Sie „git.exe“ über die Befehlszeile ausführen können.

1. Installieren Sie [CMake](https://cmake.org/download/), und stellen Sie sicher, dass Sie „cmake.exe“ über die Befehlszeile ausführen können. Empfohlen wird CMake Version 3.7.2 oder höher. Das Installationsprogramm **.msi** ist die einfachste Option unter Windows. Fügen Sie CMake für mindestens den aktuellen Benutzer zu PATH hinzu, wenn das Installationsprogramm Sie dazu auffordert.

1. Installieren Sie [Python 2.7](https://www.python.org/downloads/release/python-27). Stellen Sie sicher, dass Sie Python Ihrer `PATH`-Umgebungsvariable über **Systemsteuerung > System > Erweiterte Systemeinstellungen > Umgebungsvariablen** hinzufügen.

1. Führen Sie an einer Eingabeaufforderung den folgenden Befehl aus, um das Azure IoT Edge-GitHub-Repository auf Ihrem lokalen Computer zu klonen:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a>Erstellen des Beispiels

Sie können nun die IoT Edge-Laufzeit und Beispiele auf dem lokalen Computer erstellen:

1. Öffnen Sie eine **Entwicklereingabeaufforderung für VS 2015** oder eine **Entwicklereingabeaufforderung für VS 2017**.

1. Navigieren Sie zum Stammordner in Ihrer lokalen Kopie des Repositorys **iot-edge**.

1. Führen Sie das Skript zum Erstellen wie folgt aus:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Dieses Skript erstellt eine Visual Studio-Projektmappendatei und erstellt die Projektmappe. Sie finden die Visual Studio-Projektmappe im **build**-Ordner Ihrer lokalen Kopie des Repositorys **iot-edge**. Fügen Sie den Parameter `--run-unittests` hinzu, wenn Sie die Komponententests erstellen und ausführen möchten. Fügen Sie `--run-e2e-tests` hinzu, wenn Sie die End-to-End-Tests erstellen und ausführen möchten.

> [!NOTE]
> Bei jeder Ausführung löscht das Skript **build.cmd** den Ordner **build** im Stammordner Ihrer lokalen Kopie des Repositorys **iot-edge** und erstellt ihn neu.