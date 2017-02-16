---
title: Problembehandlung bei Ihrem Azure IoT-Gateway | Microsoft-Dokumentation
description: "Problembehandlungsseite für Intel NUC-Gateway"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: IoT-Probleme, Probleme mit dem Internet der Dinge
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/21/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: e3e4ad430d8941a09543ce2dc97f8e449a39bced
ms.openlocfilehash: c1c7679af9a931c46d25a2e1509e67b781b00f35


---
# <a name="troubleshooting"></a>Problembehandlung

## <a name="hardware-issues"></a>Hardwareprobleme

### <a name="ti-sensortag-cannot-be-connected"></a>TI SensorTag kann nicht verbunden werden

Verwenden Sie für die Problembehandlung von SensorTag-Verbindungsproblemen die [SensorTag-App](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).

### <a name="have-an-issue-with-intel-nuc"></a>Probleme mit Intel NUC

Informationen zum Beheben von Problemen beim Start finden Sie unter [Troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html) (Beheben von Problemen beim Start des Intel NUC).

Informationen zum Beheben von Problemen mit dem Betriebssystem finden Sie unter [Troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html) (Beheben von Problemen mit dem Betriebssystem auf dem Intel NUC).

Informationen zum Beheben anderer Probleme finden Sie unter [Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html) (Blinkcodes and Signaltoncodes für Intel NUC).

## <a name="nodejs-package-issues"></a>Probleme mit Node.js-Paketen

### <a name="no-response-during-gulp-tasks"></a>Keine Antwort während Gulp-Tasks

Wenn Sie während der Ausführung von Gulp-Tasks auf Probleme stoßen, können Sie die Option `--verbose` für das Debuggen hinzufügen. Versuchen Sie, laufende Gulp-Tasks mit `Ctrl + C` zu beenden, und führen Sie dann den folgenden Befehl in Ihrem Konsolenfenster aus, um Debugmeldungen anzuzeigen. In der Konsolenausgabe werden unter Umständen detaillierte Fehlermeldungen angezeigt.

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Probleme mit Device discovery (Gerätesuche)

Hilfreiche Informationen bei der Behebung von gängigen Problemen mit dem Befehl `discover-sensortag` finden Sie auf der [Wikiseite](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).

### <a name="npm-issues"></a>NPM-Probleme

Versuchen Sie, das NPM-Paket mit dem folgenden Befehl zu aktualisieren:

```bash
npm install -g npm
```

Wenn das Problem weiterhin besteht, schreiben Sie am Ende dieses Artikels einen Kommentar, oder erstellen Sie in unserem [Beispielrepository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started) ein GitHub-Problem.

## <a name="remote-debugging"></a>Remotedebugging
> Die nachfolgenden Anweisungen gelten für das Debuggen der in diesem Tutorial verwendeten Node.js-Skripts.
### <a name="run-the-sample-application-in-debug-mode"></a>Ausführen der Beispielanwendung im Debugmodus

Führen Sie den folgenden Befehl aus, um die Beispielanwendung im Debugmodus auszuführen:

```bash
gulp run --debug
```

Wenn das Debugmodul bereit ist, sollte `Debugger listening on port 5858` in der Konsolenausgabe angezeigt werden.

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a>Konfigurieren von Visual Studio Code zum Verbinden mit dem Remotegerät

1. Öffnen Sie den Bereich **Debuggen** auf der linken Seite.
2. Klicken Sie auf die grüne Schaltfläche (F5) **Debuggen starten**. Visual Studio Code öffnet eine `launch.json`-Datei.
3. Aktualisieren Sie die `launch.json`-Datei mit dem folgenden Inhalt. Ersetzen Sie `[device hostname or IP address]` durch die tatsächliche IP-Adresse oder den Hostnamen des Geräts.

   ``` json
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
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Remotedebuggen – Konfiguration](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Anfügen an die Remoteanwendung

Klicken Sie auf die grüne Schaltfläche (F5) **Debuggen starten**, um das Debuggen zu starten.

Lesen Sie [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging), um weitere Informationen über den Debugger zu erhalten.

![Debuggen der BLE-Beispielanwendung](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a>Probleme mit Azure CLI

Die Azure-Befehlszeilenschnittstelle (Azure CLI) ist ein Vorschaubuild. Sie können den [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) (Installationshandbuch für Vorabversionen) verwenden, um nach Lösungen zu suchen.

Wenn mit dem Tool Fehler auftreten, legen Sie einen [Eintrag](https://github.com/Azure/azure-cli/issues) im Bereich **Probleme** des GitHub Repositorys an.

Hilfreiche Informationen zur Behebung von gängigen Problemen finden Sie in der [Infodatei](https://github.com/Azure/azure-cli/blob/master/README.rst).

Wenn in einem Fehler angezeigt wird, dass keine Version gefunden werden konnte, die die Anforderung erfüllt, führen Sie den folgenden Befehl aus, um pip auf die neueste Version zu aktualisieren.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Probleme bei der Installation von Python

### <a name="legacy-installation-issues-macos"></a>Probleme bei der Vorgänger-Installation (MacOS)

Bei der Installation von pip tritt ein Berechtigungsfehler auf, wenn frühere Pakete vorhanden sind, die mit **su**-Berechtigungen installiert wurden. Diese Situation tritt auf, wenn eine frühere Installation von Python mithilfe von brew (MacOS) nicht vollständig deinstalliert wurde. Einige pip-Pakete aus einer früheren Installation wurden mit „root“ erstellt, wodurch der Berechtigungsfehler verursacht wird. Die Lösung ist, die Pakete zu entfernen, die durch root erstellt wurden. Führen Sie die folgenden Schritte aus, um dies abzuschließen:

1. Besuchen Sie `/usr/local/lib/python2.7/site-packages`.
2. Listen Sie die Pakete auf, die mit „root“ erstellt wurden: `ls -l | grep root`
3. Deinstallieren Sie die Pakete aus Schritt 2: `sudo rm -rf {package name}`
4. Installieren Sie Python neu.

## <a name="azure-iot-hub-issues"></a>Probleme mit Azure IoT Hub

Wenn Sie Azure IoT Hub erfolgreich mit der Azure-Befehlszeilenschnittstelle bereitgestellt haben und ein Tool benötigen, um die Geräte zu verwalten, die eine Verbindung mit IoT Hub herstellen, können Sie die unten angegebenen Tools ausprobieren.

### <a name="device-explorer"></a>Geräte-Explorer

[Geräte-Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) wird auf Ihrem lokalen Windows-Computer ausgeführt und stellt eine Verbindung mit IoT-Hub in Azure her. Er kommuniziert mit den folgenden [IoT Hub-Endpunkten](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):

- Geräteidentitätsverwaltung zum Bereitstellen und Verwalten von Geräten, die bei IoT Hub registriert sind.
- Empfangen von Gerät-zu-Cloud, damit Sie von Ihrem Gerät an IoT Hub gesendete Nachrichten überwachen können.
- Senden von Cloud-zu-Gerät ermöglicht es Ihnen, von Ihren Geräten Nachrichten an IoT Hub zu senden.

Konfigurieren Sie Ihre IoT Hub-Verbindungszeichenfolge in diesem Tool, um alle Funktionen nutzen zu können.

### <a name="iothub-explorer"></a>iothub-explorer

[iothub-explorer](https://github.com/Azure/iothub-explorer) ist ein Beispiel für CLI-Tools für mehrere Plattformen, um Geräteclients zu verwalten. Mit dem Tool können Sie Geräte im Identitätsregister verwalten, Gerät-zu-Cloud-Nachrichten überwachen und Cloud-zu-Gerät-Befehle senden.

Zum Installieren der aktuellen Version (Vorabversion) des Tools „iothub-explorer“ führen Sie den folgenden Befehl aus:

```bash
npm install -g iothub-explorer@latest
```

Zum Abrufen zusätzlicher Hilfe zu allen iothub-explorer-Befehlen und den zugehörigen Parametern führen Sie den folgenden Befehl aus:

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a>Das Azure-Portal

Sie können eine vollständige CLI-Oberfläche nutzen, um alle Azure-Ressourcen zu erstellen und zu verwalten. Sie können auch das [Azure-Portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) nutzen, um das Bereitstellen, Verwalten, und Debuggen Ihrer Azure-Ressourcen zu unterstützen.

## <a name="azure-storage-issues"></a>Probleme mit Azure Storage

[Microsoft Azure-Speicher-Explorer (Vorschau)](http://storageexplorer.com/) ist eine eigenständige App von Microsoft, mit der Sie unter Windows, MacOS und Linux mit Azure Storage-Daten arbeiten können. Mit diesem Tool können Sie eine Verbindung mit Ihrer Tabelle herstellen und die Daten darin anzeigen. Mithilfe dieses Tools können Sie Probleme mit Azure Storage beheben.



<!--HONumber=Jan17_HO2-->


