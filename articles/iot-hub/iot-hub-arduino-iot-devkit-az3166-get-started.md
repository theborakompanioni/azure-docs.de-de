---
title: "IoT DevKit in der Cloud – Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie IoT DevKit AZ3166 einrichten und eine Verbindung mit Azure IoT Hub herstellen, um in diesem Tutorial Daten an die Azure-Cloudplattform zu senden.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 122fac584ac5b954ef7b33a3121bee2c502ebc63
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a>Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub in der Cloud

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Das [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/)-Board kann zur Entwicklung von IoT-Lösungen (Internet of Things), die auf Microsoft Azure-Diensten basieren, sowie zum Erstellen eines Prototyps dieser Lösungen verwendet werden. Es enthält ein Arduino-kompatibles Board mit leistungsstarken Peripheriegeräten und Sensoren, ein Open Source-Boardpaket und einen wachsenden [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).

## <a name="what-you-do"></a>Aufgaben
Sie verbinden [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) mit einer Azure IoT Hub-Instanz, die Sie erstellen, erfassen die Temperatur- und Luftfeuchtigkeitsdaten von Sensoren und senden die Daten an IoT Hub.

Sie haben noch kein DevKit-Board? Dann beziehen Sie [hier](https://aka.ms/iot-devkit-purchase) ein neues DevKit-Board.

## <a name="what-you-learn"></a>Lerninhalt

* Herstellen einer Verbindung von IoT DevKit mit einem drahtlosen Zugriffspunkt und Vorbereiten der Entwicklungsumgebung
* Erstellen einer IoT Hub-Instanz und Registrieren eines Geräts für MXChip IoT DevKit
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf dem MXChip IoT DevKit-Board
* Senden der Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Erforderliches Element

* Ein MXChip IoT DevKit-Board mit einem Micro-USB-Kabel. [Jetzt kaufen](https://aka.ms/iot-devkit-purchase)
* Einen Computer (Windows 10 oder macOS 10.10+).
* Ein aktives Azure-Abonnement
  * [Kostenloses Microsoft Azure-Testkonto für 30 Tage](https://azureinfo.microsoft.com/us-freetrial.html) aktivieren

## <a name="prepare-your-hardware"></a>Vorbereiten der Hardware

Schließen Sie die Hardware an Ihren Computer an.

### <a name="hardware-you-need"></a>Erforderliche Hardware

* DevKit-Board
* Micro-USB-Kabel

![getting-started-hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-to-your-computer"></a>Verbinden von DevKit mit Ihrem Computer

1. Schließen Sie den USB-Stecker des Kabels an Ihren PC an.
2. Schließen Sie den Micro-USB-Stecker an DevKit an.
3. Die LED neben der Aufschrift „Power“ leuchtet grün, wodurch bestätigt wird, dass der Anschluss durchgeführt ist.

![getting-started-connect](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a>Konfigurieren von WLAN

Für IoT-Projekte ist Internetkonnektivität erforderlich. Befolgen Sie die nachstehenden Anweisungen, um DevKit für die Herstellung einer WLAN-Verbindung zu konfigurieren.

### <a name="enter-ap-mode"></a>Wechseln in den AP-Modus

Halten Sie die Taste B gedrückt, drücken Sie gleichzeitig einmal die Reset-Taste, und lassen Sie dann Taste B los. Ihr DevKit-Board wechselt zur WLAN-Konfiguration in den AP-Modus. Auf dem Display werden die SSID (Service Set Identifier) von DevKit sowie die IP-Adresse des Konfigurationsportals angezeigt:

![getting-started-wifi-ap](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a>Herstellen einer Verbindung mit DevKit im AP-Modus

Verbinden Sie nun ein anderes WLAN-fähiges Gerät (PC oder Mobiltelefon) mit der DevKit-SSID (im oben dargestellten Screenshot gekennzeichnet), und lassen Sie das Kennwort leer.

![getting-started-ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a>Konfigurieren von WLAN für DevKit

Geben Sie in den Browser Ihres PC oder Mobiltelefons die IP-Adresse ein, die auf dem DevKit-Display angezeigt wird. Wählen Sie das WLAN-Netzwerk aus, mit dem DevKit eine Verbindung herstellen soll, und geben Sie das Kennwort ein. Klicken Sie zum Abschluss auf **Verbinden**:

![getting-started-wifi-portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

Sobald die Verbindung hergestellt wird, wird DevKit innerhalb weniger Sekunden neu gestartet. Anschließend werden der WLAN-Name und die IP-Adresse auf dem Display angezeigt:

![getting-started-wifi-ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
Die im Foto angezeigte IP-Adresse entspricht möglicherweise nicht der tatsächlichen IP-Adresse, die zugewiesen ist und auf dem DevKit-Display angezeigt wird. Dies ist normal, da bei WLAN das DHCP zur dynamischen Zuweisung von IP-Adressen verwendet wird.

Nach der WLAN-Konfiguration werden Ihre Anmeldeinformationen für diese Verbindung auf dem Gerät beibehalten, auch wenn es nicht angeschlossen ist. Wenn Sie die WLAN-Konfiguration für DevKit zu Hause vorgenommen und DevKit dann zu Ihrem Arbeitsplatz mitgenommen haben, müssen Sie den AP-Modus neu konfigurieren (beginnend mit Schritt **Wechseln in den AP-Modus**), um DevKit mit dem WLAN Ihres Arbeitsplatzes zu verbinden. 

## <a name="start-using-devkit"></a>Erste Schritte mit DevKit

Die auf DevKit ausgeführte Standard-App überprüft auf die neueste Firmwareversion und zeigt einige Sensordiagnosedaten für Sie an.

### <a name="upgrade-to-the-latest-firmware"></a>Durchführen eines Upgrades auf die neueste Firmware

Wenn ein Upgrade erforderlich ist, werden sowohl die aktuelle als auch die neueste Firmwareversion auf dem Display angezeigt. Befolgen Sie zum Durchführen eines Upgrades die Anweisungen im Leitfaden zum [Durchführen eines Firmwareupgrades](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/).

![getting-started-firmware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
Dies ist eine einmalige Aufgabe. Wenn Sie mit der Entwicklung auf DevKit beginnen und Ihre App hochladen, ist die neueste Firmware für Ihre App bereits installiert.

### <a name="test-various-sensors"></a>Testen von verschiedenen Sensoren

Drücken Sie Taste B zum Testen der Sensoren, und wiederholen Sie diesen Vorgang mehrmals, um jeden Sensor zu durchlaufen.

![getting-started-sensors](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a>Vorbereiten der Entwicklungsumgebung

Nun erfolgt die Einrichtung der Entwicklungsumgebung: In dieser stehen Ihnen Tools und Pakete für die Erstellung von leistungsstarken IoT-Anwendungen zur Verfügung. Sie können die für Ihr Betriebssystem entsprechende Windows- oder macOS-Version auswählen.


### <a name="windows"></a>Windows

Es wird empfohlen, das Installationspaket zur Vorbereitung der Entwicklungsumgebung zu verwenden. Wenn Probleme auftreten, können Sie hierfür die [manuellen Schritte](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) durchführen.

#### <a name="download-latest-package"></a>Herunterladen des neuesten Pakets

Die Datei `.zip`, die Sie herunterladen, enthält alle erforderlichen Tools und Pakete für die DevKit-Entwicklung.

> [!div class="button"]
[Download](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> Die Datei `.zip` enthält die folgenden Tools und Pakete. Wenn bereits einige Komponenten installiert sind, erkennt das Skript diese und überspringt sie.
> * Node.js und Yarn: Runtime für das Setupskript und automatisierte Aufgaben
> * [MSI-Installationsprogramm für die Azure CLI 2.0](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) – Die plattformübergreife Befehlszeilenoberfläche für die Verwaltung von Azure-Ressourcen, das MSI-Installationsprogramm, mit pip, dem jeweiligen Verwaltungsprogramm für Python-Pakete
> * [Visual Studio Code](https://code.visualstudio.com/): Einfacher Codeeditor für die DevKit-Entwicklung
> * [Visual Studio Code-Erweiterung für Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Ermöglicht die Arduino-Entwicklung in Visual Studio Code
> * [Arduino IDE](https://www.arduino.cc/en/Main/Software): Das Tool, auf dem die Erweiterung für Arduino basiert
> * DevKit-Boardpaket: Toolketten, Bibliotheken und Projekte für DevKit
> * Dienstprogramm ST-Link: Wichtige Dienstprogramme und Treiber

#### <a name="run-installation-script"></a>Ausführen des Installationsskripts

Navigieren Sie im Windows-Dateiexplorer zur `.zip`-Datei, und extrahieren Sie sie. Suchen Sie dann nach der Datei `install.cmd`, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Administrator ausführen**, um sie zu starten.

![getting-started-run-admin](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

Während der Installation wird der Status jedes Tools oder Pakets angezeigt.

![getting-started-install](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-to-install-drivers"></a>Bestätigen der Treiberinstallation

Die Visual Studio Code-Erweiterung für Arduino basiert auf der Arduino-IDE. Wenn Sie die Arduino-IDE zum ersten Mal installieren, werden Sie zur Installation der relevanten Treiber aufgefordert:

![getting-started-driver](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

Die Installation dauert abhängig von der Internetgeschwindigkeit ungefähr 10 Minuten. Nach Abschluss der Installation sollten die Verknüpfungen für Visual Studio Code und die Arduino-IDE auf Ihrem Desktop angezeigt werden.

> [!NOTE] 
Gelegentlich kann es vorkommen, dass beim Starten von Visual Studio Code eine Fehlermeldung angezeigt wird, die besagt, dass die Arduino-IDE oder das jeweilige Boardpaket nicht gefunden werden kann. Um dieses Problem zu beheben, schließen Sie Visual Studio Code, und starten Sie erneut die Arduino-IDE. Visual Studio Code sollte dann ordnungsgemäß zum Pfad der Arduino-IDE navigieren.


### <a name="macos-preview"></a>macOS (Vorschau)

Führen Sie folgende Schritte durch, um die Entwicklungsumgebung unter macOS vorzubereiten.

#### <a name="install-azure-cli-20"></a>Installieren von Azure CLI 2.0

Führen Sie zur Installation der Azure CLI 2.0 die Anweisungen im [offiziellen Leitfaden](https://docs.microsoft.com//cli/azure/install-azure-cli) durch:

Installieren Sie die Azure CLI 2.0 mit einem `curl`-Befehl:

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

Starten Sie Ihre Befehlsshell neu, damit die Änderungen wirksam werden:

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a>Installieren der Arduino-IDE

Die Visual Studio Code-Erweiterung für Arduino basiert auf der Arduino-IDE. Laden Sie die [Arduino-IDE für macOS](https://www.arduino.cc/en/Main/Software) herunter, und installieren Sie sie.

#### <a name="install-visual-studio-code"></a>Installieren von Visual Studio Code

Laden Sie [Visual Studio Code für macOS](https://code.visualstudio.com/) herunter, und installieren Sie die Software. Dies ist das primäre Entwicklungstool für die Erstellung von DevKit-IoT-Anwendungen.

####  <a name="download-latest-package"></a>Herunterladen des neuesten Pakets

1. Installieren Sie Node.js. Für die Installation können Sie einen gängigen macOS-Paket-Manager wie [Homebrew](https://brew.sh/) oder ein [vordefiniertes Installationsprogramm](https://nodejs.org/en/download/) verwenden.

2. Laden Sie die `.zip`-Datei mit den Taskskripts herunter, die für die DevKit-Entwicklung in Visual Studio Code erforderlich sind.

   > [!div class="button"]
   [Download](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   Navigieren Sie zur `.zip`-Datei, und extrahieren Sie sie. Starten Sie anschließend die **Terminal**-App, und führen Sie zur Konfiguration die folgenden Befehle aus:

   Verschieben Sie den extrahierten Ordner in Ihren macOS-Benutzerordner:
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   Installieren Sie `npm`-Pakete:
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a>Installieren der Visual Studio Code-Erweiterung für Arduino

Mit Visual Studio Code können Sie direkt im Tool Marketplace-Erweiterungen installieren. Klicken Sie zur Installation im Menübereich auf der linken Seite einfach auf das Symbol für Erweiterungen, und suchen Sie dann nach `Arduino`:

![installation-extensions](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a>Installieren des DevKit-Boardpakets

Das DevKit-Board muss mithilfe des Board-Managers in Visual Studio Code hinzugefügt werden.

1. Rufen Sie mit der Tastenkombination `Cmd+Shift+P` die Befehlspalette auf, und geben Sie **Arduino** ein. Suchen Sie dann nach **Arduino: Board-Manager**, und wählen Sie ihn aus.

2. Klicken Sie unten rechts auf **Weitere URLs**.
   ![installation-additional-urls](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)

3. Fügen Sie in der Datei `settings.json` unten im Bereich `USER SETTINGS` eine Zeile hinzu, und speichern Sie sie.
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![installation-settings-json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. Suchen Sie nun im Board-Manager nach „az3166“, und installieren Sie die neueste Version.
   ![installation-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)

Nun sind alle erforderlichen Tools und Pakete für macOS installiert.


## <a name="open-project-folder"></a>Öffnen des Projektordners

### <a name="launch-vs-code"></a>Starten Sie VS Code.

Stellen Sie sicher, dass DevKit nicht verbunden ist. Starten Sie zuerst Visual Studio Code, und verbinden Sie DevKit mit Ihrem Computer. Visual Studio Code findet es automatisch, und eine Einführungspopupseite wird angezeigt:

![mini-solution-vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
Gelegentlich kann es vorkommen, dass beim Starten von Visual Studio Code eine Fehlermeldung angezeigt wird, die besagt, dass die Arduino-IDE oder das jeweilige Boardpaket nicht gefunden werden kann. Um dieses Problem zu beheben, schließen Sie Visual Studio Code, und starten Sie erneut die Arduino-IDE. Visual Studio Code sollte dann ordnungsgemäß zum Pfad der Arduino-IDE navigieren.

### <a name="open-arduino-examples-folder"></a>Öffnen des Ordners „Arduino-Beispiele“

Wechseln Sie zur Registerkarte **Arduino-Beispiele**, navigieren Sie zu `Examples for MXCHIP AZ3166 > AzureIoT`, und klicken Sie auf `GetStarted`.

![mini-solution-examples](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

Wenn Sie den Bereich geschlossen haben sollten, rufen Sie zum erneuten Laden die Befehlspalette mit der Tastenkombination `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) auf. Geben Sie **Arduino** ein, um nach **Arduino: Beispiele** zu suchen und diese Option auszuwählen.

## <a name="provision-azure-services"></a>Bereitstellen von Azure-Diensten

Führen Sie im Projektmappenfenster mit der Tastenkombination `Ctrl+P` (macOS: `Cmd+P`) Ihre Aufgabe aus, indem Sie „task cloud-provision“ eingeben:

Im Visual Studio Code-Terminal werden Sie über eine interaktive Befehlszeile durch die Bereitstellung der erforderlichen Azure-Dienste geführt:

![mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a>Erstellen und Hochladen von Arduino-Sketches

### <a name="install-required-library"></a>Installieren der erforderlichen Bibliothek

1. Drücken Sie `F1` oder `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`), um die Befehlspalette aufzurufen, und geben Sie **Arduino** ein. Suchen Sie dann nach **Arduino: Bibliothek-Manager**, und wählen Sie ihn aus.

2. Suchen Sie nach der `ArduinoJson`-Bibliothek, und klicken Sie auf **Installieren**.

### <a name="build-and-upload-the-device-code"></a>Erstellen und Hochladen des Gerätecodes

Führen Sie mit `Ctrl+P` (macOS: `Cmd+P`) den Schritt „task device-upload“ aus. Der Terminal fordert Sie zum Wechseln in den Konfigurationsmodus auf. Halten Sie hierfür Taste A gedrückt, und drücken Sie anschließend gleichzeitig einmal die Reset-Taste. Auf dem Display wird „Konfiguration“ angezeigt. Auf diesem Display wird die Verbindungszeichenfolge festgelegt, die im Schritt „task cloud-provision“ abgerufen wird.

Daraufhin wird mit der Überprüfung und dem Upload des Arduino-Sketches begonnen:

![mini-solution-device-upload](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

DevKit wird neu gestartet und beginnt mit der Ausführung des Codes.

## <a name="test-the-project"></a>Testen des Projekts

Klicken Sie in Visual Studio Code in der Statusleiste auf das Symbol für den Netzstecker, um den seriellen Monitor zu öffnen.

Die Beispielanwendung wird erfolgreich ausgeführt, wenn folgende Ergebnisse angezeigt werden:

* Der serielle Monitor zeigt dieselben Informationen an wie den unten stehenden Inhalt im folgenden Screenshot.
* Die LED auf dem MXChip IoT DevKit-Board blinkt.

![Endgültige Ausgabe in Visual Studio Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), oder wenden Sie sich über folgende Kanäle an uns.

## <a name="next-steps"></a>Nächste Schritte

Sie haben ein MXChip IoT DevKit-Board mit IoT Hub verbunden und die erfassten Sensordaten an IoT Hub gesendet.

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

- [Verwalten von Cloudgerät-Nachrichten mit iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [Speichern von IoT Hub-Nachrichten im Azure-Datenspeicher](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Power BI](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [Visualisieren von Sensordaten in Azure IoT Hub in Echtzeit mit Azure-Web-Apps](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [Weather forecast using the sensor data from your IoT hub in Azure Machine Learning](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning) (Wettervorhersage mithilfe von Sensordaten Ihres IoT Hub in Azure Machine Learning)
- [Geräteverwaltung mit iothub-explorer](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [Remoteüberwachung und Benachrichtigungen mit Logic Apps](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)
