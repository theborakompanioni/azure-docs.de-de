---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Problembehandlung | Microsoft-Dokumentation"
description: "Problembehandlungsseite für die Node.js-Umgebung von Raspberry Pi"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: IoT-Probleme, Probleme mit dem Internet der Dinge
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 2c5e2955584b165ca0ca95ce424a2050fb3f6ccd
ms.lasthandoff: 01/24/2017


---
# <a name="troubleshooting"></a>Problembehandlung
## <a name="hardware-issues"></a>Hardwareprobleme
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>Die Anwendung wird ordnungsgemäß ausgeführt, aber die LED blinkt nicht
Dieses Problem bezieht sich immer auf die Hardwareverbindung. Führen Sie die folgenden Schritte aus, um die Probleme zu identifizieren:

1. Überprüfen Sie, ob Sie für Ihre Platine die richtige **GPIO**-Wahl getroffen haben. Die beiden Ports sollten **GPIO GND (Pin 6)** und **GPIO 04 (Pin 7)** lauten.
2. Überprüfen Sie, ob die Polarität der LED korrekt ist. Das längere Bein (Anodenanschluss) sollte auf die **positive** Anoden-pin hinweisen.
3. Verwenden Sie **3.3V Pin** und **GND Pin** auf dem Raspberry Pi 3. Behandeln Sie den Pi als Gleichstromgerät. Überprüfen Sie, ob die LED richtig funktioniert.

![LED-Spezifikation](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andere Hardwareprobleme
Informationen zum Beheben von gängigen Problemen auf dem Raspberry Pi 3 finden Sie auf der [offiziellen Problembehandlungsseite](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Probleme mit Node.js-Paketen
### <a name="no-response-during-gulp-tasks"></a>Keine Antwort während Gulp-Tasks
Wenn Sie während der Ausführung von Gulp-Tasks auf Probleme stoßen, können Sie die Option `--verbose` für das Debuggen hinzufügen. Versuchen Sie, den laufenden Gulp-Task mit STRG+C zu beenden, und führen Sie dann den folgenden Befehl in Ihrem Konsolenfenster aus, um Debugmeldungen anzuzeigen. In der Konsolenausgabe werden unter Umständen detaillierte Fehlermeldungen angezeigt.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Probleme mit Device discovery (Gerätesuche)
Hilfreiche Informationen bei der Behebung von gängigen Problemen mit dem Befehl `devdisco` finden Sie in der [Infodatei](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="npm-issues"></a>NPM-Probleme
Versuchen Sie, das NPM-Paket mit dem folgenden Befehl zu aktualisieren:

```bash
npm install -g npm
```

Wenn das Problem weiterhin besteht, schreiben Sie am Ende dieses Artikels einen Kommentar, oder erstellen Sie in unserem [Beispielrepository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started) ein GitHub-Problem.

## <a name="remote-debugging"></a>Remotedebuggen
### <a name="run-the-sample-application-in-debug-mode"></a>Ausführen der Beispielanwendung im Debugmodus
```bash
gulp run --debug
```

Wenn das Debugmodul bereit ist, sollte ```Debugger listening on port 5858``` in der Konsolenausgabe angezeigt werden.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Konfigurieren von Visual Studio Code zum Verbinden mit dem Remotegerät
1. Öffnen Sie den Bereich **Debuggen** auf der linken Seite.
2. Klicken Sie auf die grüne Schaltfläche (F5) **Debuggen starten**. Visual Studio Code öffnet die Datei „launch.json“.
3. Aktualisieren Sie die Datei „launch.json“ mit dem folgenden Inhalt. Ersetzen Sie `[device hostname or IP address]` durch die tatsächliche IP-Adresse oder den Hostnamen des Geräts.

> [!NOTE]
> Weitere Informationen zum Visual Studio-Debuggen finden Sie im Artikel zum [Debuggen in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).


```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Remotedebuggen – Konfiguration](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Anfügen an die Remoteanwendung
Klicken Sie auf die grüne Schaltfläche (F5) **Debuggen starten**, um das Debuggen zu starten.

Lesen Sie [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging), um weitere Informationen über den Debugger zu erhalten.

![Interaktiv remotedebuggen](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Probleme mit Azure CLI
Die Azure-Befehlszeilenschnittstelle (Azure CLI) ist ein Vorschaubuild. Sie können den [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) (Installationshandbuch für Vorabversionen) verwenden, um nach Lösungen zu suchen.

Wenn mit dem Tool Fehler auftreten, legen Sie einen [Eintrag](https://github.com/Azure/azure-cli/issues) im Bereich **Probleme** des GitHub Repositorys an.

Hilfreiche Informationen zur Behebung von gängigen Problemen finden Sie in der [Infodatei](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Probleme bei der Installation von Python
### <a name="legacy-installation-issues-macos"></a>Probleme bei der Vorgänger-Installation (MacOS)
Bei der Installation von pip tritt ein Berechtigungsfehler auf, wenn frühere Pakete vorhanden sind, die mit **su**-Berechtigungen installiert wurden. Diese Situation tritt auf, wenn eine frühere Installation von Python mithilfe von brew (MacOS) nicht vollständig deinstalliert wurde. Einige pip-Pakete aus einer früheren Installation wurden mit „root“ erstellt, wodurch der Berechtigungsfehler verursacht wird. Die Lösung ist, die Pakete zu entfernen, die durch root erstellt wurden. Führen Sie die folgenden Schritte aus, um dies abzuschließen:

1. Wechseln Sie zu „/usr/local/lib/python2.7/site-packages“.
2. Listen Sie die Pakete auf, die mit „root“ erstellt wurden: `ls -l | grep root`
3. Deinstallieren Sie die Pakete aus Schritt 2: `sudo rm -rf {package name}`
4. Installieren Sie Python neu.

## <a name="azure-iot-hub-issues"></a>Probleme mit Azure IoT Hub
Wenn Sie Azure IoT Hub erfolgreich mit der Azure CLI bereitgestellt haben und ein Tool benötigen, um die Geräte zu verwalten, die eine Verbindung mit IoT Hub herstellen, können Sie die unten angegebenen Tools ausprobieren.

### <a name="device-explorer"></a>Geräte-Explorer
Der [Geräte-Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) wird auf Ihrem lokalen Windows-Computer ausgeführt und stellt eine Verbindung mit Ihrer IoT Hub-Instanz in Azure her. Er kommuniziert mit den folgenden [IoT Hub-Endpunkten](iot-hub-devguide.md):


* *Geräteidentitätsverwaltung* zum Bereitstellen und Verwalten von Geräten, die bei IoT Hub registriert sind.
* *Empfangen von Gerät-zu-Cloud*, damit Sie von Ihrem Gerät an IoT Hub gesendete Nachrichten überwachen können.
* *Senden von Cloud-zu-Gerät* ermöglicht es Ihnen, von Ihren Geräten Nachrichten an IoT Hub zu senden.

Konfigurieren Sie Ihre IoT Hub-Verbindungszeichenfolge in diesem Tool, um alle Funktionen nutzen zu können.

### <a name="iothub-explorer"></a>iothub-explorer
[iothub-explorer](https://github.com/Azure/iothub-explorer) ist ein Beispiel für CLI-Tools für mehrere Plattformen zur Verwaltung von Geräteclients. Mit dem Tool können Sie Geräte in der Identitätsregistrierung verwalten, Gerät-zu-Cloud-Nachrichten überwachen und Cloud-zu-Gerät-Nachrichten senden.

Um die aktuellste Version (Vorabversion) des iothub-explorer-Tools zu installieren, führen Sie den folgenden Befehl in Ihrer Befehlszeilenumgebung aus:

```bash
npm install -g iothub-explorer@latest
```

Sie können den folgenden Befehl verwenden, um weitere Hilfe zu allen iothub-explorer-Befehlen und ihren Parametern zu erhalten:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure-Portal
Sie können eine vollständige CLI-Oberfläche nutzen, um alle Azure-Ressourcen zu erstellen und zu verwalten. Sie können auch das [Azure-Portal](../azure-portal-overview.md) nutzen, um das Bereitstellen, Verwalten, und Debuggen Ihrer Azure-Ressourcen zu unterstützen.

## <a name="azure-storage-issues"></a>Probleme mit Azure Storage
[Microsoft Azure-Speicher-Explorer (Vorschau)](http://storageexplorer.com) ist eine eigenständige App von Microsoft, mit der Sie unter Windows, MacOS und Linux mit Azure Storage-Daten arbeiten können. Mit diesem Tool können Sie eine Verbindung mit Ihrer Tabelle herstellen und die Daten darin anzeigen. Mithilfe dieses Tools können Sie Probleme mit Azure Storage beheben.


